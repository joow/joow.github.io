---
layout: post
title: Prolonger la validité de vos clés PGP
---

Une bonne pratique pour la gestion de vos clés PGP est de créer une clé principale sans date d'expiration et de la 
conserver hors ligne stockée de manière sécurisée (clé USB chiffrée par exemple).  
Pour une utilisation quotidienne des sous-clés sont créées, chacune ayant une seule finalité (signature, chiffrement ou 
authentification).
Ces clés ont elles une date d'expiration relativement courte (6 mois ou 1 an), ce qui réduit les risques en cas de 
perte et "force" à remanipuler un peu GPG, qui n'est pas vraiment facile d'accès.

Il faut donc penser régulièrement à prolonger la validité des sous-clés.  
Une autre possibilité serait de générer de nouvelles sous-clés, c'est un peu plus sécurisé car cela permet de rendre 
impossible l'exploitation d'anciens messages avec ces nouvelles clés mais cela oblige également à chiffrer à nouveau en 
utilisant la nouvelle sous-clé de chiffrement.

## Installation de Tails

La première étape est d'installer la distribution [Tails] sur une clé USB. Cela permet de faire toutes les opérations 
sur un système plutôt sûr.  
En fonction de votre modèle de menace vous pouvez bien sûr décider d'utiliser votre système habituel.

Une fois [Tails] installé, démarrez sur la clé USB et pensez à activer le mode root afin de pouvoir installer les 
paquets nécessaires :

    sudo apt update && sudo apt install gnupg2 gnupg-agent \
        cryptsetup scdaemon pcscd yubikey-personalization \
        dirmngr secure-delete

Vous pouvez vous déconnecter du réseau lorsque l'installation des paquets supplémentaires est terminée afin de 
manipuler vos clés PGP hors ligne.

## Import des clés

Afin de manipuler vos clés il faut les réimporter dans GnuPG.

Pour cela branchez votre clé USB chiffrée contenant la sauvegarde de votre clé principale et montez la :

    sudo cryptsetup open /dev/sdc1 usb
    sudo mkdir -p /mnt/encrypted-usb
    sudo mount /dev/mapper/usb /mnt/encrypted-usb

Créez un répertoire afin de stocker les données de GnuPG :

    export GNUPGHOME=$(mktemp -d)
    cd $GNUPGHOME

Importez vos clés et récupérez la configuration de GnuPG :

    gpg --import /mnt/encrypted-usb/<tmp.XXX>/mastersub.key
    cp -v /mnt/encrypted-usb/<tmp.XXX>/ $GNUPGHOME

Editez votre clé :

    export KEYID=0x<ID>
    gpg --edit-key --expert $KEYID

Pour chacune des sous-clés (signature, chiffrement et authentification) utilisez la commande `expire` afin de prolonger 
la date de validité :

    key 1
    expire
    1y
    # Déselectionnez la clé précédemment sélectionnée, la commande expire ne fonctionne que sur une clé à la fois
    key 1
    key 2
    expire
    ...
    # Sauvegardez les modifications effectuées
    save

Exportez la clé principale et les sous-clés et sauvegardez les sur la clé USB chiffrée :

    gpg --armor --export-secret-keys $KEYID > $GNUPGHOME/mastersub.key
    gpg --armor --export-secret-subkeys $KEYID > $GNUPGHOME/sub.key
    sudo cp -avi $GNUPGHOME >/mnt/encrypted-usb

Vous pouvez éventuellement supprimer sur la clé USB chiffrée l'ancien répertoire `tmp.XXX` qui contient la sauvegarde 
précédente :

    sudo rm -rf /mnt/encrypted-usb/<tmp.XXX>

Démontez la clé USB :

    sudo umount /mnt/encrypted-usb
    sudo cryptsetup close usb

Exportez la clé publique :

    sudo mkdir /mnt/public
    sudo mount /dev/sdb2 /mnt/public
    gpg --armor --export $KEYID | sudo tee /mnt/public/$KEYID-$(date +%F).txt
    sudo umount /mnt/public

Il ne reste plus qu'à transférer les nouvelles sous-clés sur votre clé YubiKey si vous en utilisez une :

    gpg --edit-key --expert $KEYID
    key 1
    keytocard
    key 1
    key 2
    keytocard
    ...
    save

Vous pouvez enfin redémarrer et stockez en lieu sûr votre clé USB chiffrée pour la ressortir l'année prochaine.
