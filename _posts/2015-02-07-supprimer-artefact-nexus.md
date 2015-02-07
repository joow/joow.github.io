---
layout: post
title: Supprimer un artefact de Nexus
---
Il peut parfois être nécessaire de supprimer manuellement un artefact de Nexus, par exemple lorsque les metadata renvoyées par Nexus sont incorrectes et indiquent une mauvaise version alors que l'on cherche à obtenir la dernière version de développement (_-SNAPSHOT_).

La commande suivante permet de supprimer l'artefact ayant comme _id_ **webapp**, comme _groupId_ **com.my.company** et comme _version_ **0.9.9-SNAPSHOT** :

	curl --request DELETE --user <user>:<password> --write-out "%{http_code} %{url_effective}\\n" --output /dev/null --silent <url nexus>/service/local/repositories/<repository>/content/com/my/company/webapp/0.9.9-SNAPSHOT

Il est également nécessaire de supprimer les metadata associées afin de forcer Nexus à les reconstruire :

	curl --request DELETE --user <user>:<password> --write-out "%{http_code} %{url_effective}\\n" --output /dev/null --silent <url nexus>/service/local/metadata/repositories/<repository>/content/com/my/company/webapp