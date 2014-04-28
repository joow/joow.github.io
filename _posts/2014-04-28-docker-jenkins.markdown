---
layout: post
title: Docker, la virtualisation poids plume
---
[Docker] est en ce moment le mot à la mode dans le monde de la virtualisation et du développement, et plus particulièrement dans le monde Devops.

Il s'agit d'un outil de virtualisation légère. Légère car il s'appuie sur le principe des conteneurs légers pour virtualiser n'importe quelle application, en s'appuyant sur le système d'exploitation sous-jacent et ses resources. A la différence des logiciels comme VirtualBox ou VMWare, il n'émule pas un système complet (matériel et logiciel) mais virtualise uniquement ce qui est propre à chaque logiciel virtualisé, en partageant toutes les autres ressources.
Le site officiel propose un très bon tutoriel en ligne expliquant les grands principes de Docker et son utilisation.
Bien qu'il soit possible de monter un container en ligne de commande, la meilleure manière de créer un container est d'utiliser un fichier décrivant le container à instancier (une sorte de recette) nommé `Dockerfile`.

Voici par exemple un fichier `Dockerfile` permettant de créer un container [Jenkins], un outil d'intégration continue très utilisé dans le monde Java :

	FROM centos
	RUN yum -y install wget
	RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
	RUN rpm --import http://pkg.jenkins-ci.org/redhat-stable/jenkins-ci.org.key
	RUN yum -y install java-1.7.0-openjdk-devel jenkins
	
	ENTRYPOINT ["java", "-jar", "/usr/lib/jenkins/jenkins.war"]
	
	USER jenkins

	EXPOSE 8080

Cette recette part d'une base [CentOS], installe wget, ajoute le dépôt stable de Jenkins, installe le JDK 7 et Jenkins. Enfin le serveur Jenkins est démarré avec l'utilisateur `jenkins` et le port 8080 (utilisé par défaut par Jenkins) est exposé.
A partir de cette recette il ne reste plus qu'à construire le container en le nommant (optionnel mais fortement conseillé) :

	docker build -t jenkins .

Et à démarrer le conteneur :

	docker run -P jenkins

En utilisant la commande `docker ps` vous pourrez voir quel port est utilisé sur la machine hôte et correspond au port 8080 du container. Il ne reste plus qu'à accéder à votre serveur Jenkins à l'adresse `http://localhost:<port_hote>`.

Vous vous apercevrez rapidement que le temps nécessaire pour construire et démarrer le container est très rapide (moins de cinq minutes si vous avez une bonne connexion Internet) !
Par contre si vous vous amusez un peu à configurer votre serveur Jenkins, l'arrêtez puis le redémarrez vous vous rendrez compte que toute votre configuration a été perdue, Docker créant des containers sans état. Il est bien entendu possible de contourner cela sans difficulté mais je n'ai pas étudié cette possibilité.

Je suis très intéressé par Docker, je trouve que c'est un moyen simple de tester rapidement quelque chose, de jeter, de recommencer, tout en ayant un système beaucoup plus léger que les systèmes de virtualisation "classiques" comme VirtualBox ou VMWare. Le seul inconvénient de Docker est qu'il nécessite obligatoire Linux puisqu'il s'appuie sur les conteneurs légers implémentés par Linux, les développeurs ou opérationnels sous Mac ou Windows devront donc passer d'abord par la virtualisation d'un système Linux complet !

Vous pouvez retrouver le fichier `Dockerfile` pour Jenkins sur mon compte Github sur le [dépôt dédié] et l'utiliser pour construire le container :

	docker build -t jenkins github.com/joow/docker-jenkins

[Docker]: https://www.docker.io/
[Jenkins]: http://jenkins-ci.org/
[CentOS]: http://www.centos.org/
[dépôt dédié]: https://github.com/joow/docker-jenkins