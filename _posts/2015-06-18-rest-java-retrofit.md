---
layout: post
title: Transformer une API REST en code Java avec Retrofit
---
Après plusieurs mois de silence, je me décide enfin à écrire un nouveau billet afin de vous présenter une librairie Java dont on commence à entendre parler : [Retrofit].  
Elle permet de créer une interface Java représentant une API REST et ensuite appeler l'API au travers de cette interface Java.

## Exemple

[Retrofit] permet de transformer une API REST en interface Java avec quelques annotations :

{% highlight java %}
import retrofit.http.*;

interface Service {
	@GET("/")
	String getIndex();
}
{% endhighlight %}

Cette interface permet de déclarer une méthode `getIndex()` qui va renvoyer l'index d'un service donné.

Il suffit ensuite de générer l'implémentation du service avec la classe `RestAdapter` :
{% highlight java %}
final RestAdapter restAdapter = new RestAdapter.Builder()
    .setEndpoint("http://www.service.com")
    .build();

final Service service = restAdapter.create(Service.class);
final String index = service.getIndex();
{% endhighlight %}

Comme vous pouvez le voir il est très simple de générer l'implémentation et ainsi exploiter l'API dans notre code Java.  
Bien entendu [Retrofit] supporte plusieurs [verbes] du protocole HTTP (`@POST`, `@PUT`, `@DELETE` et `@HEAD`)

[Retrofit] supporte aussi l'utilisation d'URLs paramétrées :
{% highlight java %}
@GET("/data/{year}/{month}/{day}")
String getData(@Path("year") String year, @Path("month") String month, @Path("day") String day);
{% endhighlight %}

ainsi que l'ajout de paramètres à une URL (/data?year=\<year>&month=\<month>&day=\<day>) :
{% highlight java %}
@GET("/data")
String getData(@Query("year") String year, @Query("month") String month, @Query("day") String day);
{% endhighlight %}
## Formulaire

Il est également très simple d'utiliser un formulaire à l'aide de [Retrofit] :

{% highlight java %}
@FormUrlEncoded
@POST("/login")
String login(@FieldMap Map<String, String> user, @Field("PL_CSRF_TOKEN") String csrfToken);

{% endhighlight %}

Grâce à l'implémentation générée par la classe `RestAdapter` il est ainsi très simple de s'authentifier auprès de notre service utilisant un formulaire :

{% highlight java %}
Map<String, String> user = new HashMap<>();
user.put("username", "nom");
user.put("password", "mdp");

final String login = service.login(user, "csrfToken");
{% endhighlight %}

Ici le formulaire nécessite trois paramètres :
	* le nom d'utilisateur et le mot de passe, on les fournit grâce à une `Map`
	* le jeton [CSRF] passé directement en indiquant le nom du paramètre dans le formulaire.

Il est également possible d'utiliser l'annotation `@Body` afin de fournir un objet comme body pour la requête HTTP.

## Conversion

Par défaut [Retrofit] convertit la réponse en JSON (en utilisant la librairie [Gson]). Il fournit également des convertisseurs pour les formats XML et [Protocol Buffers]. Bien entendu il est possible de créer son propre convertisseur. Par exemple ici on va créer un convertisseur HTML :
{% highlight java %}
class HtmlConverter implements Converter {
    @Override
    public String fromBody(TypedInput body, Type type) throws ConversionException {
        final List<String> lines = new ArrayList<>();

        try (final BufferedReader reader = new BufferedReader(new InputStreamReader(body.in()))) {
            String line;
            while ((line = reader.readLine()) != null) {
                lines.add(line);
            }
        } catch (IOException e) {
            throw new ConversionException(e);
        }

        return lines.stream().collect(Collectors.joining(System.lineSeparator()));
    }

    @Override
    public TypedOutput toBody(Object object) {
        throw new UnsupportedOperationException();
    }
}

// Utiliser le convertisseur HTML
final RestAdapter restAdapter = new RestAdapter.Builder()
    .setEndpoint("http://www.service.com")
    .setConverter(new HtmlConverter())
    .build();
{% endhighlight %}

Enfin il est possible d'obtenir directement la réponse brute et de l'exploiter par la suite en déclarant que la méthode renvoie une `Response` :
{% highlight java %}
Response login(@FieldMap Map<String, String> user, @Field("PL_CSRF_TOKEN") String csrfToken);
{% endhighlight %}

## Cookies

Par défaut le client HTTP du JDK est utilisé. Pour utiliser un client plus avancé comme [OkHttp] il suffit d'ajouter la dépendance. Ce client peut par exemple permettre de gérer de manière simple l'utilisation de cookies :
{% highlight java %}
final CookieManager cookieManager = new CookieManager();
cookieManager.setCookiePolicy(CookiePolicy.ACCEPT_ALL);
final OkHttpClient httpClient = new OkHttpClient();
httpClient.setCookieHandler(cookieManager);

final RestAdapter restAdapter = new RestAdapter.Builder()
    .setEndpoint("http://www.service.com")
    .setClient(new OkClient(httpClient))
    .build();
{% endhighlight %}

## Conclusion

[Retrofit] est une librairie qui permet d'exploiter de manière élégante une API REST en Java. Je n'ai pas abordé toutes ses possibilités mais sachez qu'elle permet de couvrir la plupart des API REST en supportant par exemple l'ajout d'entêtes (`@Headers`), l'upload de fichiers (annotations `@Multipart` et `@Part`), la possibilité d'ajouter des intercepteurs ou encore la possibilité de faire des requêtes asynchrones.  

La prochaine fois que vous avez à exploiter une API REST, pensez à [Retrofit] avant d'envisager dimplémenter une solution maison.

[Retrofit]: http://square.github.io/retrofit/
[verbes]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
[CSRF]: http://www.wikiwand.com/fr/Cross-Site_Request_Forgery
[Gson]: https://code.google.com/p/google-gson/
[Protocol Buffers]: https://developers.google.com/protocol-buffers/
[OkHttp]: http://square.github.io/okhttp/