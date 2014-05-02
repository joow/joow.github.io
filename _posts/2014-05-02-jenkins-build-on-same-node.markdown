---
layout: post
title: Déclencher une tâche sur le même nœud Jenkins
---

Certaines tâches Jenkins doivent en déclencher d'autres une fois terminées avec succès, par exemple dans le cas où l'on souhaite publier sur un site les résultats de tests, publier des métriques ou par exemple propager les derniers changements au code source vers un autre référentiel, ...

Lorsque votre serveur Jenkins utilise plusieurs nœuds, il peut arriver que l'on souhaite forcer l'exécution de la tâche suivante sur le même nœud, lorsque cette seconde tâche nécessite les ressources du répertoire de travail de la première tâche. Pour cela il faut d'abord installer le plugin [Parameterized Trigger Plugin] si ce n'est pas déjà fait. Ce plugin permet de déclencher d'autres tâches en lui passant différents types de paramètres, dont l'un est "Build on the same node". Ce paramètre permettra de forcer la tâche déclenchée à s'exécuter sur le même nœud que la première tâche.

Une capture d'écran valant mieux qu'un long discours :

<img src="../../../images/jenkins-build-on-the-same-node.png" alt="Ajouter le paramètre Build on the same node" height="100%" width="100%"/>

[Parameterized Trigger Plugin]: https://wiki.jenkins-ci.org/display/JENKINS/Parameterized+Trigger+Plugin
