---
layout: post
title: Critique du livre Gradle in Action
---

## Présentation
[Gradle in Action] est un livre sorti récemment (février 2014) aux éditions [Manning] présentant l'outil de build [Gradle].
Il est divisé en trois parties :

1. Introduction à Gradle (*Introducing Gradle*)
2. Maîtriser les fondamentaux (*Mastering the fundamentals*)
3. De la construction au déploiement (*From build to deployment*)

Ces trois parties présentent une utilisation très complète de Gradle et leur lecture est linéaire, il est donc conseillé de les lire dans l'ordre.

### Partie 1 : Introduction à Gradle
La première partie revient sur l'automatisation de la construction, pourquoi cela est vivement conseillée et quelles sont les grands outils utilisés dans le monde Java ([Ant] et [Maven]). Elle présente les avantages de Gradle sur ces outils puis explique comment installer Gradle. Enfin le projet qui sera utilisé tout au long du livre est expliqué ainsi qu'un premier exemple d'utilisation de Gradle.
Cette première partie est une excellente introduction à Gradle. Elle a été raccourcie par rapport aux premières versions du livre qui insistaient un peu trop sur les avantages et qualités (indéniables) de Gradle. Cette partie peut sembler un peu longue pour les personnes connaissant déjà Ant et Maven mais me semble intéressante et nécessaire pour les personnages ne connaissant pas très bien les outils d'automatisation de construction.

### Partie 2 : Maîtriser les fondamentaux
Cette deuxième partie rentre très rapidement dans le vif du sujet en expliquant le fonctionnement d'un script de construction Gradle, les tâches et comment interagir avec le cycle de construction d'un projet. La gestion des dépendances, la construction multi-projets, les tests (unitaires, d'intégrations et fonctionnels) ainsi que la migration d'un script existant (Ant ou Maven) sont autant de sujets abordés dans cette partie. Enfin un chapitre complet est dédié au développement d'un plugin Gradle.
Cette deuxième partie est pour moi le coeur du livre et elle est indispensable pour toute personne désirant maîtriser Gradle (d'où le nom de cette partie !). Les exemples sont très nombreux et les explications très claires. De plus l'auteur a choisi d'utiliser tout au long du livre un même projet assez simple (application ToDo) ce qui permet de se focaliser sur la construction du projet en lui-même. Néanmoins petit à petit des éléments sont introduits dans ce projet (JavaScript, Selenium), le rendant proche de la réalité. Si vous ne devez lire qu'une partie du livre lisez celle-ci.

### Partie 3 : De la construction au déploiement
La dernière partie du livre pousse l'utilisation de Gradle afin de montrer comment l'utiliser avec un IDE, construire des projets contenant différents langages (Java, Groovy, Scala, JavaScript), comment gérer la qualité des projets (de Checkstyle à Sonar en passant par JaCoCo). Ces différentes parties permettent de décrire l'utilisation des différents plugins existants pour Gradle. Bien que ces parties ne soient pas nécessaires pour la compréhension de Gradle vous pourrez toujours vous y référer si besoin. Pour finir cette dernière partie montre comment intégrer Gradle dans un processus de déploiement continu : intégration avec Jenkins, publication d'artefacts sur Artifactory et enfin provisionnement et déploiement sur différents environnements. Cette partie sera très appréciée par les personnes s'intéressant au mouvement [DevOps] et présentera aux autres les avantages de ce type de solution.

## Conclusion
J'avais déjà lu par le passé un premier livre sur Gradle. Très court (80 pages) celui-ci ne s'était révélé être qu'au final une formulation différente de la documentation disponible sur le site de Gradle (excellente au passage). Du coup j'attendais avec impatience un livre bien plus complet sur Gradle qui permettrait de bien comprendre le fonctionnement de Gradle et comment le mettre en oeuvre pour automatiser la construction de différents projets. C'est clairement un excellent livre. Il est très complet, les explications sont claires et détaillées. L'exemple utilisé tout au long du livre bien que simple est finalement assez riche pour couvrir la plupart des aspects d'une application moderne et ainsi démontrer l'utilisation de Gradle avec de nombreux outils modernes. Par contre cela peut être un peu trop riche car le livre couvre la plupart du cycle de vie de la construction d'un projet moderne : de sa compilation à son déploiement (même dans le cloud !). Le lecteur peu au fait de tous ces aspects pourra se sentir perdu mais comme je l'ai dit la deuxième partie du livre est la seule réellement nécessaire pour bien appréhender Gradle. Les différents chapitres de la troisième partie pourront être lues ponctuellement pour répondre à un besoin.
Bref je recommande chaudement cet excellent livre sur Gradle. A noter que l'auteur (**Benjamin Muschko**) est un développeur travaillant pour la société derrière Gradle (Gradleware), ce qui explique sa connaissance de l'outil.

[Gradle in Action]: http://www.manning.com/muschko/
[Manning]: http://www.manning.com/
[Gradle]: http://www.gradle.org/
[Ant]: http://ant.apache.org/
[Maven]: http://maven.apache.org/
[DevOps]: http://devops.fr/
