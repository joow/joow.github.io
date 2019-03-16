---
layout: post
title: Aller à la pêche aux identifiants avec evilginx2
---

[evilginx2] est un outil diabolique permettant de récupérer des identifiants d'authentification.

Un lien malveillant est donné à la victime, [evilginx2] faisant office de proxy entre le site cible et la victime afin d'intercepter les identifiants de connexion ([attaque de l'homme du milieu]).  
Même l'utilisation de la double authentification ne permet pas de pallier cette attaque, evilginx2 étant capable de récupérer également les cookies de sessions.

**AVERTISSEMENT**

**Il est strictement illégal de voler les identifiants d'un utilisateur.**  
**[evilginx2] peut être utilisé pour mener des campagnes de sensibilisation auprès des utilisateurs, après accord écrit et signé de ceux-ci ou de leur direction informatique.**

Ce billet montre l'utilisation de l'outil avec la distribution [Kali](TODO LINK), en mode local, utile pour des fins de test.  
evilginx2 est utilisable sur n'importe quelle distribution Linux ainsi que sur Windows.

Une fois la distribution Kali démarrée téléchargez la version Linux (x86) disponible ici : [https://github.com/kgretzky/evilginx2/releases](https://github.com/kgretzky/evilginx2/releases)  
Ouvrez un terminal afin de décompresser l'archive puis installez l'outil :

    sh install.sh

Nous allons démarrer l'outil en mode développeur, vu que nous allons uniquement travailler en local.  
Ce mode permet d'utiliser des certificats auto-signés.  
En mode "réel", evilginx2 est capable de demander automatiquement des certificats signés par [Let's Encrypt].
Par contre il faut que vous disposiez d'un nom de domaine et que vous redirigiez ses serveurs de noms sur l'adresse IP de la machine faisant tourner evilginx2.

Lancer evilginx2, en mode développeur :

    evilginx -developer

La première chose à faire est de configurer le nom de domaine et l'adresse IP à utiliser :

    config domain phishme.com
    config ip 127.0.0.1

evilginx2 propose plusieurs "phislets", des fichiers de configuration au format YAML, permettant de configurer les URLs à surveiller.  
Par défaut plusieurs sites cibles sont disponibles, comme par exemple LinkedIn, Twitter ou encore Facebook.

Dans notre cas nous allons cibler Reddit.
Pour cela nous déclarons un sous-domaine cible :

    phislets hostname reddit reddit.phishme.com

Notez bien qu'un attaquant utilisera un domaine plus discret que phishme.com :smiley:  
Il nous faut récupérer les URLs à déclarer localement afin qu'elles soient résolues sur notre machine.  
Cette opération ne serait pas utile en mode réel :

    phislets get-urls reddit

Les lignes fournies par evilginx2 doivent être ajoutées dans le fichier `/etc/hosts`, cela nous permettra de résoudre localement notre site de hameçonnage.

Il faut enfin créer un "appât" avec la commande `lures` :

    lures create reddit
    lures get-url 0 # 0 correspond à l'identifiant fourni par la commande précédente.

L'URL fournie par la dernière commande est à transmettre à notre victime.  
En général l'attaquant utilisera un raccourcisseur d'URLs et l'enverra par email à la victime, en se faisant passer pour une connaissance (il existe bien entendu bien d'autres vecteurs d'attaque possibles !).

Afin de tester l'attaque il ne nous reste plus qu'à ouvrir Firefox, importer le certificat racine de evilginx2 (ce ne serait pas utile en mode réel, le certificat de notre site de hameçonnage aurait été signé par Let's Encrypt) et naviguez sur l'URL malveillante.  
Le certificat racine est disponible normalement dans le répertoire `~/.evilginx/crt/ca.crt`

Comme vous pourrez le voir le site reproduit fidèlement la cible, puisque evilginx2 fait office de proxy !

Si vous vous connectez vous pourrez récupérer les identifiants de connexions et les cookies de session en utilisant la commande `sessions` :

    sessions id
    sessions <ID>

Pour se protéger de ce type d'attaques il faut bien faire attention aux URLs que l'on saisit !
Et cela semble plus difficile qu'il n'y paraît, qui n'a jamais cliqué sur un lien transmis par une connaissance par SMS ou email, ou un lien sur une chaîne Slack ou Discord !

Une autre solution est d'utiliser une clé [U2F] ou la spécification [WebAuthentication] qui permet d'éviter les attaques par hameçonnage.

Enfin l'utilisation de certains gestionnaires de mots de passe et de leur intégration aux navigateurs peut également vous protéger, le remplissage automatique ne marchant pas, l'origine du site ne correspondant pas 
(l'origine d'un site correspond au protocole, http ou https, le nom de domaine pleinement qualifié et éventuellement le port).

Surtout restez vigilant et ne cliquez pas trop facilement sur des liens !

---
[evilginx2]: https://github.com/kgretzky/evilginx2
[attaque de l'homme du milieu]: https://fr.wikipedia.org/wiki/Attaque_de_l%27homme_du_milieu
[Let's Encrypt]: https://letsencrypt.org/
[U2F]: https://fr.wikipedia.org/wiki/Universal_Second_Factor
[WebAuthentication]: https://developer.mozilla.org/en-US/docs/Web/API/Web_Authentication_API
