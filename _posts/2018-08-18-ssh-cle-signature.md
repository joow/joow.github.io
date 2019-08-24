---
title: Authentification SSH par clé publique signée
layout: post
---

Il existe deux moyens répandus de se connecter à un serveur SSH :
  
  1. par mot de passe
  2. par clé publique

Le premier moyen est clairement déconseillé, le second est celui généralement recommandé.

Il existe également un autre moyen, moins répandu mais tout aussi sécurisé.  
Ce type d'authentification se base également sur la cryptographie asymétrique mais utilise une autorité de 
certification afin de signer les clés publiques des utilisateurs souhaitant s'identifier.  
Cela permet de ne pas nécessiter la copie de la clé publique de chaque utilisateur sur les serveurs. Il faudra à la 
place faire signer la clé publique des utilisateurs pouvant se connecter.

## Configuration du serveur

Du côté du serveur il faut commencer par créer la clé de l'autorité de certification qui sera utilisée pour signer les 
clés des utilisateurs pouvant se connecter.

En tant qu'utilisateur root :

  1. Générer la clé : `ssh-keygen -t ed25519 -a 100 -f ssh_users_ed25519_key` (ou 
     `ssh-keygen -b 4096 -o -a 100 -f ssh_users_ed25519_key` si vous utiliser une version d'OpenSSH 
     inférieure à 6.5)
  2. Copier la clé privée et la clé publique dans le répertoire du serveur ssh : `/etc/ssh`
  2. Copier également la clé publique sur les autres serveurs SSH
  3. Éditer le fichier de configuration du serveur OpenSSH (`/etc/ssh/sshd_config`) afin d'ajouter la ligne suivante :
     `TrustedUserCAKeys /etc/ssh/ssh_users_ed25519_key.pub`
  4. Recharger la configuration du serveur OpenSSH : `systemctl reload sshd`

## Configuration du client

Il reste maintenant à signer la clé publique du client souhaitant se connecter.

Pour cela toujours sur le serveur lancer la commande suivante :

    ssh-keygen -s /etc/ssh/ssh_users_ed25519_key -I <user>@<server> -n <user> -V +365d id_ed25519.pub

`-s` permet d'indiquer la clé à utiliser pour la signature (celle générée dans le paragraphe précédent).  
`-I` permet d'indiquer l'identifiant du certificat, qui sera utilisé pour les journaux. C'est une chaîne arbitraire.  
`-n` permet de restreindre l'utilisateur pouvant utiliser cette clé. Il est possible d'indiquer plusieurs utilisateurs 
(séparés par une virgule) ou de ne pas préciser l'option ; dans ce cas tous les utilisateurs pourront se connecter avec le certificat  
`-V` permet d'indiquer la durée de validité du certificat (ici 1 an).  
`id_ed25519.pub` est la clé publique à signer.

Vous pouvez vérifier les informations du certificat en utilisant la commande suivante :

```shell
ssh-keygen -L -f id_ed25519-cert.pub
```

Il ne reste plus qu'à copier le certificat signé (`id_ed25519-cert.pub`) sur les clients pouvant se connecter à l'aide 
du certificat et à éventuellement supprimer la clé publique de l'utilisateur du fichier `authorized_keys`.
