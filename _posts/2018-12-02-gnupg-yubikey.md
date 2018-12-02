---
layout: post
title: Générer une clé PGP avec Tails et l'importer sur une YubiKey
---

[PGP], ou plutôt [OpenPGP], est un standard de chiffrement pour les communications, en particulier utilisé pour les emails.

Nous allons voir dans ce billet comment utiliser [GnuPG], une implémentation libre du standard [OpenPGP], pour générer 
une clé et l'importer ensuite sur une clé [YubiKey].

Ce billet est inspiré de cet excellent [guide](https://github.com/drduh/YubiKey-Guide) que je vous invite à lire.

## Installation de Tails

Il est vivement recommandé d'utiliser une distribution Live pour générér la clé afin d'éviter tout risque.  
La meilleure solution est d'installer [Tails].

Pour vérifier l'ISO téléchargé n'oubliez pas d'importer la clé utilisée pour signer la signature d'image ISO :

    gpg --import tails-signing.key
    gpg --no-options --keyid-format 0xlong --verify tails-amd64-3.10.1.iso.sig tails-amd64-3.10.1.iso

Une fois [Tails] installée démarrez en activant l'accès root (en ajoutant une option supplémentaire sur l'écran de 
configuration de [Tails]), cela vous permettra d'installer les paquets nécessaires.

Mettez à jour la liste des paquets et installez les paquets nécessaires :

    sudo apt-get update
    sudo apt-get install -y \
        gnupg2 gnupg-agent \
        cryptsetup scdaemon pcscd \
        yubikey-personalization \
        dirmngr \
        secure-delete

Vérifiez que l'entropie de votre système soit suffisante :

    cat /proc/sys/kernel/random/entropy_avail

Une valeur proche de 4096 est vivement recommandé, vu que nous allons générer des clés d'une longueur de 4096 bits.  
Si ce n'est pas le cas essayez d'installer rng-tools.

## Création des clés

Générez un répertoire de travail temporaire pour [GnuPG] :

    export GNUPGHOME=$(mktemp -d); echo $GNUPGHOME

Téléchargez le fichier de configuration de [GnuPG] disponible [ici](https://raw.githubusercontent.com/drduh/config/master/gpg.conf).  
Utilisez le navigateur Tor pour le télécharger, vous n'y aurez pas accès directement avec `wget`.

Vous pouvez désormais vous déconnecter du réseau pour la suite.

### Clé principale

Nous allons tout d'abord créer la clé principale.  
Elle sera uniquement utilisée pour certifier les autres clés que nous utiliserons au quotidien.

Créez une nouvelle clé RSA de signature d'une longueur de 4096 bits sans date d'expiration et entrez vos informations personnelles.

Exportez l'identifiant de la clé afin de pouvoir le réutiliser plus tard :

    export KEYID=0x<ID>

### Sous-clés

Nous allons ensuite générer trois sous-clés d'une longueur de 4096 bits et d'une durée de validité d'un an (vérifiez 
que votre [YubiKey] supporte bien cette longueur, les clés NEO ne supportent que des clés d'une taille de 2048 bits 
maximum).  
**Pensez à noter dans votre agenda de renouveller vos sous-clés dans 11 mois**

Éditez votre clé maîtresse :

    gpg --expert --edit-key $KEYID

Demandez à ajouter une clé RSA de signature avec la commande `addkey`.

Créez ensuite une clé RSA de chiffrement en utilisant à nouveau `addkey`.

Enfin créez une clé RSA d'authentification, toujours en utilisant `addkey`.  
**Attention** : [GnuPg] ne propose pas la possibilité de créer une clé d'authentification directement.
Il faut donc indiquer que l'on souhaite créer une clé RSA avec des possibilités personnalisées.  
Par défaut [GnuPG] proposera une clé de signature et de chiffrement. Désactivez ces possibilités et activez à la place l'authentification.

Enfin sauvegardez vos changements avec la commande `save`.

Vérifiez que vos quatre clés sont bien présentes :

    gpg --list-secret-keys

Enfin vérifiez la bonne configuation de vos clés :

    gpg --export $KEYID | hokey lint

## Sauvegarde des clés

Nous allons à présent sauvegarder les clés sur une clé USB chiffrée.

Commencez par les exporter :

    gpg --armor --export-secret-keys $KEYID > $GNUPGHOME/mastersub.key
    gpg --armor --export-secret-subkeys $KEYID > $GNUPGHOME/sub.key

Puis branchez une clé USB vide, effacez sa table de partition avec `fdisk` :

    sudo fdisk /dev/sdc
    o (pour créer une table de partitions DOS)
    w (pour sauvegarder et quitter)

Réinsérez la clé USB et créez une partition :

    sudo fdisk /dev/sdc
    n (laissez les options par défaut, partition primaire n°1 occupant tout l'espace disponible)
    w

Chiffrez la partition avec [LUKS] :

    sudo cryptsetup luksFormat /dev/sdc1

Montez la partition :

    sudo cryptsetup luksOpen /dev/sdc1 usb

Et créez un système de fichiers :

    sudo mkfs.ext4 /dev/sdc1 -L usb (permet de nommer la clé "usb")

Enfin montez la partition et copiez le répertoire de [GnuPG] :

    sudo mount /dev/sdc1 /mnt
    sudo cp -avi $GNUPGHOME /mnt

Terminez en démontant la partition :
    
    sudo umount /mnt
    sudo cryptsetup luksClose usb

## Configuration de la YubiKey

Insérez votre [YubiKey] et éditez la en tant que smart card :

    gpg --card-edit

La première étape à réaliser est de renseigner les codes PIN utilisateur et administrateur :

    admin
    passwd
    3 (pour changer le PIN administrateur par défaut, 12345678)
    1 (pour changer le PIN utilisateur par défaut, 123456)
    q (pour quitter)
    name (pour renseigner le "surname", nom de famille, et le "given name", le prénom)
    lang (pour renseigner la langue, fr par exemple)
    login (pour indiquer l'adresse email)
    quit

Éditez votre clé :

    gpg --edit-key $KEYID

Sélectionnez la première clé, celle de signature :

    key 1

Et exportez la sur la carte :

    keytocard

_note : la clé exportée sur la carte sera supprimée de votre trousseau [GnuPG]_

Désélectionnez la première clé, sélectionnez la deuxième, celle de chiffrement, et exportez la :

    key 1
    key 2
    keytocard

Enfin exportez la troisième clé, celle d'authentification et sauvegardez :

    key 2
    key 3
    keytocard
    save

Vérifiez que les clés ont bien été exportées sur la [YubiKey] :

    gpg --list-secret-keys

_les clés privées sont indiquées par le libellé `ssb>`_

## Nettoyage

Exportez votre clé publique sur une autre clé USB :

    gpg --armor --export $KEYID > /mnt/public-usb-key/pubkey.txt

Vérifiez que les points suivants ont bien été faits :

  - Sauvegarde de la clé de signature, de chiffrement et d'authentification sur la [YubiKey]
  - Mémorisation des codes PIN de la [YubiKey] (utilisateur et administrateur)
  - Mémorisation de la phrase de passe de la clé principale
  - Sauvegarde de la clé principale et des sous-clés sur une clé USB chiffrée
  - Mémorisation de la phrase de passe de la clé USB chiffrée
  - Sauvegarde de la clé publique

Supprimez de manière sécurisée le répertoire de travail de [GnuPG] :

    sudo srm -r $GNUPGHOME

Redémarrez et lancez votre système d'exploitation habituel.

## Utilisation des clés

Configurez à nouveau [GnuPG] (en utilisant le fichier d'exemple disponible [ici](https://github.com/drduh/config/blob/master/gpg.conf)).

Importez votre clé publique :

    gpg --import /mnt/pubkey.txt

Puis éditez là afin d'indiquer que vous faites confiance à cette clé avec le niveau maximum :

    gpg --edit-key 0x<ID>
    trust
    5
    save

Insérez votre [YubiKey] et vérifiez son statut :

    gpg --card-status

Le libellé `sec#` indique que la clé principale n'est pas disponible.  
C'est normal puisqu'elle doit uniquement être conservée sur votre clé USB chiffrée.

## Chiffrement

Vous pouvez tester le chiffrement d'un message avec la clé publique :

    echo "Hello, world!" | gpg --encrypt --armor --recipient 0x<ID>

Puis son déchiffrement :

    gpg --decrypt --armor

Cette fois le code PIN de votre [YubiKey] sera demandée.

## Signature

Vous pouvez également tester la signature d'un message avec la clé de signature :

    echo "Hello, world!" | gpg --armor --clearsign --default-key 0x<ID SIGN>

Et vérifier la signature :

    gpg (copiez la signature puis CTRL+D)

## Authentification

Pour terminer vous pouvez également utiliser la clé d'authentification afin de vous authentifier à SSH en utilisant l'agent fourni par [GnuPG].  
L'article mentionné en début de billet contient toutes les informations pour sa mise en place.

---
[PGP]: https://fr.wikipedia.org/wiki/Pretty_Good_Privacy
[OpenPGP]: https://www.openpgp.org/
[GnuPG]: https://gnupg.org/
[Tails]: https://tails.boum.org/
[YubiKey]: https://www.yubico.com/
[LUKS]: [https://fr.wikipedia.org/wiki/LUKS]
