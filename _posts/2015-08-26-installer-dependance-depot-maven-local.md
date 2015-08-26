---
layout: post
title: Installer une dépendance dans le dépôt Maven local
---
Il peut parfois être utile d'installer manuellement une dépendance de votre
projet dans votre dépôt Maven local (par exemple quand votre dépôt Nexus fait
des siennes :wink:).  
Pour cela il suffit d'utiliser la commande suivante :

    mvn install:install-file -DgroupId=<groupId> -DartifactId=<artifactId> -Dversion=<version> -Dpackaging=<packaging> -Dfile=<file> -Dclassifier=<classifier> -DgeneratePom=true

où :

1. `<groupId>`, `<artifactId>` et `<version>` sont égales aux valeurs respectives utilisées pour déclarer la dépendance.
2. `<packaging>` représenter le type (par exemple `zip`). Il peut être omis pour les fichiers de type JAR.
3. `classifier` permet de classer la dépendance (javadoc, sources, package-binary, package-nonbinary, ...). S'il s'agit d'une librairie Java de type JAR cette information est facultative.
4. Enfin `<file>` est le fichier à copier dans le dépôt local.
