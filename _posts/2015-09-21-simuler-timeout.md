---
layout: post
title: Simuler un timeout
---
Il peut parfois être utile de simuler un timeout lors d'une connexion à un serveur distant.  
Pour cela il suffit d'utiliser une adresse IP non routable, comme par exemple l'adresse `10.255.255.1`.  

Ainsi l'exemple suivant échouera après 30 secondes avec une exception de type `java.net.SocketTimeoutException` (**attention**, par défaut le timeout est égal à 0, donc infini !) :

{% highlight java %}
try {
  URL url = new URL("http://10.0.0.1");
  HttpURLConnection connection = (HttpURLConnection) url.openConnection();
  connection.setConnectTimeout((int) TimeUnit.SECONDS.toMillis(30));
  System.out.println(connection.getResponseMessage());
  connection.disconnect();
} catch (IOException e) {
  e.printStackTrace();
}
{% endhighlight %}