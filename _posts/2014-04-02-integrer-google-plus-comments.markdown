---
layout: post
title: Intégrer le système de commentaire Google+ sur votre site
---

Lorsque vous souhaitez intégrer des commentaires sur votre site il est possible de développer votre propre système de commentaires.
Il est également possible voire préférable d'utiliser un service de commentaires, vous évitant ainsi de développer votre propre système. Il existe par exemple [Disqus] qui est beaucoup utilisé. Vous pouvez également utiliser le service d'un réseau social comme [Facebook] ou [Google+]. Afin d'intégrer Google+ il suffit d'ajouter les lignes suivantes à votre page en remplaçant `<URL>` par l'URL de la page :
{% highlight html %}
<div class="g-comments"
	data-href="<URL>"
    data-width="642"
    data-first_party_property="BLOGGER"
    data-view_type="FILTERED_POSTMOD">
</div>
<script src="https://apis.google.com/js/plusone.js"></script>
{% endhighlight %}

[Disqus]: http://disqus.com/
[Facebook]: https://www.facebook.com/
[Google+]: https://plus.google.com/