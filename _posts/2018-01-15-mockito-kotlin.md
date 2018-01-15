---
layout: post
title: Utiliser Mockito avec Kotlin
---

[Mockito](http://site.mockito.org/) est une excellente librairie de mocking pour Java.  

Si vous essayez de l'utiliser avec Kotlin vous risquez de rencontrer cette erreur :

{% highlight kotlin %}
val service = mock(Service::class.java)

org.mockito.exceptions.base.MockitoException: 
Cannot mock/spy class Service
Mockito cannot mock/spy because :
 - final class
{% endhighlight %}

En effet toutes les classes en Kotlin sont fermées à l'héritage par défaut. Mockito ne peut donc pas étendre la classe.  
Heureusement depuis la version 2 de Mockito il existe une solution simple :smiley:  
Pour cela il suffit de créer le fichier `resources/mockito-extensions/org.mockito.plugins.MockMaker` avec comme seul contenu la ligne suivante :

    mock-maker-inline

Désormais Mockito pourra créer sans problème des mocks de vos classes Kotlin sans avoir à les marquer comme `open` :+1: