---
title: Importer les identifiants OATH sur une YubiKey
layout: post
---

Si vous souhaitez importez vos identifiants [OATH](https://openauthentication.org/) (HOTP ou TOTP) d'une application 
vers une clé YubiKey, il est éventuellement possible d'automatiser cet import dans le cas où l'application que vous 
utilisez propose un export (idéalement _chiffré_).

C'est le cas par exemple de l'excellente application [Aegis Authenticator](https://getaegis.app/) sur Android, qui 
permet d'exporter sa base de données, de manière chiffrée par défaut (ce qui est recommandé).

Une fois la base de données chiffrée exportée il est possible d'utiliser ce 
[script](https://github.com/beemdevelopment/Aegis/blob/master/scripts/decrypt.py) Python pour la déchiffrer en local :

    python decrypt.py --input ./aegis_export.json

Vous devrez bien entendu saisir votre mot de passe et obtiendrez la liste de vos identifiants au format JSON, dont la 
structure est décrite dans ce [document](https://github.com/beemdevelopment/Aegis/blob/master/docs/vault.md#content) :

    {
        "version": 1,
        "entries": [
            {
                "type": "totp",
                "uuid": "01234567-89ab-cdef-0123-456789abcdef",
                "name": "Bob",
                "issuer": "Google",
                "icon": null,
                "info": {
                    "secret": "ABCDEFGHIJKLMNOPQRSTUVWXYZ234567",
                    "algo": "SHA1",
                    "digits": 6,
                    "period": 30
                }
            }
        ]
    }

De son côté [Yubico](https://www.yubico.com) propose la commande 
[ykman](https://support.yubico.com/support/solutions/articles/15000012643-yubikey-manager-cli-ykman-user-manual) pour 
gérer sa clé YubiKey et la sous-commande `oath` pour gérer les identifiants de type OATH.

Si vous souhaitez faire le ménage sur votre clé au cas où vous y auriez déjà stocker des identifiants OATH vous pouvez 
utiliser la commande suivante :

    ykman oath reset

**Attention ! Cette commande effacera tous les identifiants OATH stockés actuellement sur votre clé YubiKey !**

La commande `ykman oath add` permet de stocker un identifiant OATH sur la clé.

En utilisant l'outil [jq](https://stedolan.github.io/jq/) il est possible d'importer facilement tous les identifiants 
de la base de l'application Aegis sur votre clé YubiKey :

    python decrypt.py --input ./aegis_export.json | jq -c --raw-output '.entries[] | "\"--oath-type " + (.type | ascii_upcase) + " --digits \(.info.digits) --algorithm \(.info.algo) --issuer \'\(.issuer)\' --period \(.info.period) --touch --force \'\(.name)\' \'\(.info.secret)\'\""' | xargs -I % sh -c 'ykman oath add %'

Cette ligne de commande permet d'assez facilement convertir la base de données de Aegis en commande pour `ykman`.

_Notes_ : 

  1. `xargs -I %` permet de remplacer le caractère `%` dans la commande fournie à `xargs` par le contenu de l'entrée 
     standard et de considérer le saut de ligne en entrée comme séparateur, générant ainsi une commande 
     `ykman oath add ...` par ligne
  2. `sh -c` est utilisé afin de pouvoir correctement exécuter la commande `ykman ` avec le résultat fourni par `jq`. 
     Sans cela la commande `ykman oath add` serait invoquée avec le résultat de `jq` comme une seule option, provoquant 
     une erreur
  3. il est possible que certaines commandes `ykman oath add... `ne fonctionnent pas, le nom étant trop long
