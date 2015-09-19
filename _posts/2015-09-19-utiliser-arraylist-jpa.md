---
layout: post
title: Utiliser une ArrayList avec JPA
---
La spécification [JPA] requiert uniquement le support de collections définies par leur interface ([java.util.Collection], [java.util.Set], [java.util.List] et [java.util.Map]) au sein d'une entité. La définition d'une collection par le biais de son implémentation (par exemple [java.util.ArrayList]) n'est donc pas requis :

> Collection-valued persistent fields and properties must be defined in terms of one of the following collection-valued interfaces regardless of whether the entity class otherwise adheres to the JavaBeans method conventions noted above and whether field or property access is used: java.util.Collection, java.util.Set, java.util.List [3] , java.util.Map. The collection implementation type may be used by the application to initialize fields or properties before the entity is made persistent. Once the entity becomes managed (or detached), subsequent access must be through the interface type.

_page 25 de la [spécification] JPA_

En fonction de l'implémentation utilisée il est néanmoins possible que cela soit supporté.  

Voyons ce que les deux principales implémentations de [JPA] permettent :

- [EclipseLink] supporte effectivement l'utilisation d'implémentations mais avertit que cela n'est pas optimal :

{% highlight java %}
[EL Warning]: metadata: 2015-09-19 15:36:59.093--ServerSession(2066114348)--Element [field emails] within entity class [class org.joow.jpa.User] uses a collection type [class java.util.ArrayList] when the JPA specification only supports java.util.Collection, java.util.Set, java.util.List, or java.util.Map.  This type is supported with eager loading; using lazy loading with this collection type requires additional configuration and an IndirectContainer implementation that extends [class java.util.ArrayList] or setting the mapping to use basic indirection and the type to be ValueholderInterface.
{% endhighlight %}

En effet [EclipseLink] utilisera sa propre implémentation de la collection afin d'optimiser le chargement de la collection par chargement paresseux.

- [Hibernate] ne permet pas l'utilisation d'implémentations (le message d'erreur n'est pas très parlant !) :

{% highlight java %}
Illegal attempt to map a non collection as a @OneToMany, @ManyToMany or @CollectionOfElements: org.joow.jpa.User.emails
{% endhighlight %}

**Attention** donc si vous changez d'implémentation JPA (par exemple lors d'une migration du serveur d'applications).  
Vous pouvez trouver le code source du projet utilisé pour cet article sur [GitHub].

[JPA]: https://jcp.org/en/jsr/detail?id=338
[java.util.Collection]: https://docs.oracle.com/javase/8/docs/api/java/util/Collection.html
[java.util.Set]: https://docs.oracle.com/javase/8/docs/api/java/util/Set.html
[java.util.List]: https://docs.oracle.com/javase/8/docs/api/java/util/List.html
[java.util.Map]: https://docs.oracle.com/javase/8/docs/api/java/util/Map.html
[java.util.ArrayList]: https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html
[spécification]: http://download.oracle.com/otndocs/jcp/persistence-2_1-fr-eval-spec/index.html
[EclipseLink]: http://www.eclipse.org/eclipselink/
[Hibernate]: http://hibernate.org/orm/
[GitHub]: https://github.com/joow/jpa-arraylist