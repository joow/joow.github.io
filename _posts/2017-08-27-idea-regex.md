---
layout: post
title: Utiliser les expressions régulières dans IntelliJ IDEA
---

[IntelliJ IDEA] permet d'utiliser des expressions régulières afin de rechercher et remplacer du texte.  
Il permet également de référencer le texte trouvé afin de le formater différement lors du remplacement.

Par exemple supposons que nous ayons un fichier contenant des dates formatées ainsi :

    2017-01-01 00:00:00

Nous souhaitons transformer ces dates au format standard [ISO-8601](https://fr.wikipedia.org/wiki/ISO_8601) :

    2017-01-01T00:00:00Z

Il est bien sûr possible de le faire à la main pour quelques dates, mais si votre fichier en contient des milliers ce n'est pas envisageable. On pourrait aussi écrire un programme, ce serait certainement assez rapide mais à mon avis moins que d'utiliser efficacement les expressions régulières.

Pour cela il nous suffit de :

  1. décomposer l'expression d'origine
  2. référencer les différentes parties de l'expression d'origine par des variables
  3. utiliser ces variables pour construire l'expression de remplacement.

Ici l'expression d'origine est :
    
    (?<date>\d{4}-\d{2}-\d{2}) (?<time>\d{2}:\d{2}:\d{2})

On a deux groupes : `\d{4}-\d{2}-\d{2}` pour la date et `\d{2}:\d{2}:\d{2}` pour l'heure (les groupes sont délimités par les parenthèses).  
Chacun des groupes est nommé afin de pouvoir réutiliser les valeurs correspondantes dans l'expression de remplacement. La syntaxe utilisée est celle de la classe [Pattern](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) du JDK : `?<name>`. Il aurait également été possible d'omettre le nommage des groupes et de les référencer plutôt par leur index, en commençant par 1.

L'expression de remplacement est quant à elle la suivante :

    ${date}T${time}Z

ou sans nommer les groupes (pas d'accolades cette fois pour délimiter l'index) :

    $1T$2Z

De plus [IntelliJ IDEA] propose une prévisualisation du résultat, très utile pour voir si notre expression régulière fonctionne :smiley:

[IntelliJ IDEA]: https://www.jetbrains.com/idea/
