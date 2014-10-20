---
layout: post
title: Automatiser un déploiement
---
À l'heure où l'on parle beaucoup du mouvement [DevOps] il est intéressant de regarder au sein de son propre projet le mécanisme utilisé pour le déploiement et comment l'automatiser au maximum.

Ce billet décrit comment un déploiement a pu être entièrement automatisé, là où auparavant plusieurs étapes manuelles étaient nécessaires.

## Contexte
Pour commencer voici une description des étapes d'un déploiement :

1. Télécharger sur [Nexus] la dernière version *RELEASE*.
2. Copier l'archive sur le serveur cible du déploiement.
3. Décompresser cette archive, arrêter Tomcat, lancer le script d'installation, redéployer le [WAR] et redémarrer Tomcat.
4. Créer un fichier MD5 pour l'archive.
5. Uploader l'archive et le fichier MD5 sur un serveur spécifique.

Comme on peut le voir le déploiement en lui-même est assez simple mais très répétitif et il n'a rien de réellement passionnant.

Voyons comment passer de ces cinq grandes étapes manuelles à ... une (qui se résume à lancer toute la procédure !).

## Télécharger sur [Nexus]
Le téléchargement automatisé d'une archive sur [Nexus] est tout à fait possible, ce dernier offrant une [API] pour obtenir un artefact spécifique en utilisant une adresse de ce type :
	
	<URL NEXUS>/service/local/artifact/maven/content?g=<GROUP>&a=<ARTIFACT>&v=<VERSION>&r=<REPOSITORY>&p=<PACKAGING>&c=<CLASSIFIER>

À l'aide d'un outil comme [curl] il devient très facile d'automatiser le téléchargement d'une archive sur [Nexus] :

	curl -u <user>:<password> <url> ><fichier>

Quelques point sont à noter :

- Il est impossible pour `curl` (ou n'importe quel autre outil) de connaître le nom de l'archive à télécharger (il est soit déduit de l'adresse soit fourni par le serveur). Du coup par défaut ce nom sera déduit de l'adresse fournie, ce qui va donner quelque chose comme *content?a=...*, loin d'être idéal. Pour remédier à cela `curl` offre deux options intéressantes (`curl` offre beaucoup d'options intéressantes et surtout utiles comme nous le verrons tout au long de ce billet) :
	1. `--remote-header-name` (ou `-J`) qui indique à `curl` d'utiliser le nom fourni par le serveur dans les en-têtes HTTP. Malheureusement cette option n'est disponible que dans les versions récentes de `curl`. Si comme moi la version que vous utilisez ne supporte pas cette option vous allez devoir récupérer les en-têtes et déterminer le nom du fichier à l'aide de `grep` et autres `cut` !
	2. `--remote-name` (ou `-O`) qui permet de sauvegarder la réponse dans un fichier portant le nom du fichier du serveur.
- Il est fortement déconseillé d'utiliser la constante `LATEST` pour demander à [Nexus] la dernière version car vous risquez d'obtenir une version autre que celle attendue. Vous pouvez consulter ce [billet] pour plus d'informations.

## Copier l'archive à déployer
La deuxième étape ne pose pas énormément de problème.  
Il suffit d'effectuer la copie à l'aide de la commande `scp` en ayant au préalable mis en place l'authentification par clé publique.

## Déployer l'archive
Cette étape permet le déploiement de l'application. Il faut utiliser cette fois `ssh` qui permet d'exécuter des commandes à distances.  
Attention si vous devez utiliser la commande `sudo`, car de nombreux serveurs sont configurés de manière à ne pas autoriser l'exécution de `sudo` à distance (ligne `Defaults requiretty` dans le fichier `SUDOERS`). Il faut dans ce cas ajouter le paramètre `-t` (voire `-tt` pour insister) à la commande `ssh` afin de forcer `ssh` à allouer un pseudo-terminal à la session.

## Créer un fichier [MD5]
Avant de créer un fichier [MD5] pour l'archive téléchargée il peut être intéressant de vérifier l'intégrité de l'archive. Pour cela il est possible de récupérer la somme de contôle [SHA-1] sur [Nexus] (voir cette [documentation]) puis de lancer la commande `sha1sum`.  
**Attention** là encore quelques subtilités ([les joies du shell] !). Il faut utiliser la commande `echo` avec le paramètre `-e` (qui active l'interprétation du caractère antislash, bien qu'après quelques recherches il soit déconseillé d'utiliser `echo` dans un script et de préférer l'utilisation de `printf`) et ne pas oublier de mettre **deux** espaces entre la somme SHA-1 et le nom du fichier.

## Uploader un fichier
Là encore la commande `curl` nous vient en aide. Le serveur sur lequel nous devons effectuer l'upload de l'archive et du fichier [MD5] requiert une authentification. Comme beaucoup de mécanisme d'authentification elle se base sur un [cookie] et Firefox (ou un autre navigateur) va nous permettre d'obtenir facilement l'URL à utiliser pour s'authentifier (l'onglet "Réseau" permet de voir les requêtes effectuées et offre même une option "Copier comme cURL" !). Il suffit ensuite de demander à [curl] de sauvegarder le cookie :

	curl --cookie-jar /tmp/cookie.txt --data "user=<USER>&pass=<PASSWORD>" <URL> >/dev/null

Une fois authentifié on peut désormais uploader le fichier. Là encore l'utilisation judicieuse de notre navigateur nous permet d'obtenir les informations nécessaires. L'upload d'un fichier utilisant souvent un formulaire, l'option `--form` est la plus adaptée pour cette tâche (`file` étant ici le nom du champ permettant de renseigner le fichier à transmettre dans le formulaire) :

	curl --cookie /tmp/cookie.txt --request POST --form file=@<FICHIER> >/dev/null

## Configurer correctement [ssh]
Lors de mes premiers essais la connexion au serveur distant était trois fois plus longue sur le serveur contrôlant le déploiement que sur mon poste !  
Après quelques recherches (l'option `-vv` pour rendre `ssh` très bavard est votre amie ici) il s'est avéré que le serveur essayait tout d'abord d'utiliser l'authentification par *[gssapi-with-mic]* et que `ssh` attendait très longtemps avant d'abandonner cette méthode et de passer aux méthodes suivantes. En ajoutant la ligne suivante à la configuration du serveur cible il est possible de forcer la méthode à utiliser en priorité et ainsi de repasser à un temps de connexion raisonnable.

	PreferredAuthentications publickey

## Pour finir
Une fois tout ceci fait, il ne reste plus qu'à rendre simple l'exécution automatisée de toutes ces étapes par n'importe quel développeur simple. Si vous avez à votre disposition un serveur d'intégration comme [TeamCity] ou [Jenkins] cela semble le meilleur endroit pour ajouter une tâche à exécuter manuellement (la seule étape manuelle restante). Il est également possible d'aller encore plus loin, [TeamCity] permettant par exemple de lancer une tâche lorsqu'un artefact change. En le configurant correctement on peut déployer automatiquement à chaque fois qu'une nouvelle *release* est disponible sur [Nexus].

Et voilà comment on passe d'un déploiement fastidieux comportant de nombreuses étapes manuelles à un déploiement entièrement automatisé :-)

[DevOps]: http://devops.fr/
[Nexus]: http://www.sonatype.com/nexus
[WAR]: http://docs.oracle.com/javaee/7/tutorial/doc/packaging003.htm
[API]: https://oss.sonatype.org/nexus-restlet1x-plugin/default/docs/rest.html
[curl]: http://curl.haxx.se/
[billet]: http://articles.javatalks.ru/articles/32
[MD5]: http://fr.wikipedia.org/wiki/MD5
[SHA-1]: http://fr.wikipedia.org/wiki/SHA-1
[les joies du shell]: http://xkcd.com/1168/
[documentation]: https://oss.sonatype.org/nexus-restlet1x-plugin/default/docs/path__artifact_maven_resolve.html
[cookie]: http://fr.wikipedia.org/wiki/Cookie_%28informatique%29
[ssh]: http://www.openssh.com/
[gssapi-with-mic]: http://fr.wikipedia.org/wiki/GSS-API
[TeamCity]: https://www.jetbrains.com/teamcity/
[Jenkins]: http://jenkins-ci.org/