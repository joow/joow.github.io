---
title: Sécuriser son email
layout: post
---

L'email n'est pas à l'origine un protocole très sécurisé.  
Si la communication pour l'envoi d'emails est désormais généralement chiffrée, par défaut rien n'empêche d'envoyer un 
email en se faisant passer pour quelqu'un. C'est d'ailleurs quelque chose de plus en plus utilisé pour des campagnes de 
hameçonnage par exemple.

Heureusement il est possible de sécuriser sa messagerie en combinant plusieurs approches.

## SPF

[SPF] (Sender Policy Framework) permet de configurer les serveurs autorisés à envoyer des emails pour un nom de domaine 
donné.  
Pour cela il suffit d'ajouter un enregistrement DNS de type TXT (dans mon cas j'utilise le fournisseur [Runbox]) :

    "v=spf1 redirect=spf.runbox.com ~all"

En fonction de votre fournisseur d'emails cette configuration changera.  
Le `~all` à la fin indique de marquer en erreur (mais pas de rejeter) les emails ne satisfaisant pas la règle.

Il est important de noter que [SPF] doit être implémenté côté receveur afin de vérifier que le message reçu a bien été 
envoyé par un serveur autorisé.

## DKIM

[DKIM] (DomainKeys Identified Mail) utilise la cryptographie à clé publique pour signer les emails envoyés.  
Comme pour [SPF] une entrée DNS est ajoutée, permettant de publier la clé publique à utiliser pour vérifier les 
messages envoyés.  
La clé privée est elle conservée bien à l'abri côté serveur d'envoi.

La configuration va dépendre de votre serveur de messagerie.  
En général il proposera plusieurs entrées spécifiques redirigeant vers votre clé publique du moment afin de pouvoir 
faire tourner régulièrement la clé privée utilisée.

Par exemple [Runbox](https://help.runbox.com/dkim-signing/) utilise alternativement deux clés, il faudra donc ajouter 
deux entrées DNS redirigeant vers les clés :

    selector1._domainkey 3600 IN CNAME selector1-<DOMAIN>.domainkey.runbox.com.
    selector2._domainkey 3600 IN CNAME selector2-<DOMAIN>.domainkey.runbox.com.

[Fastmail](https://www.fastmail.com/help/receive/domains-advanced.html#dkimconfig) de son côté propose de mettre en 
place 3 paire de clés.

## DMARC

[DMARC] (Domain-based Message Authentication, Reporting and Conformance) est la dernière pièce du puzzle.

Ce mécanisme permet de configurer la manière dont seront gérés et reportés les emails d'un domaine par les serveurs 
receveurs.
Cela permet par exemple d'indiquer s'il faut rejeter ou non les emails non conformes à SPF et/ou DKIM et qui prévenir 
en cas de problème.
En tant qu'expéditeur vous contrôlez ainsi comment sont gérés les messages de votre domaine qui ne seraient pas 
conformes et vous pouvez être prévenu d'éventuels problèmes de configuration ou de tentatives d'usurpation.

Encore une fois il faut ajouter une entrée DNS, de type TXT avec comme nom `_dmarc.<DOMAIN>` et un TTL de 1800 :

    v=DMARC1; p=none; rua=mailto:<EMAIL ADDRESS>

Il est conseillé de commencer par ne rien faire pour les emails invalides (`p=none`) puis de passer en mode 
`quarantine` avec un certain pourcentage (ajoutez `pct=1`, puis passez à 5, 10, 25, 50 et 100) pour enfin passer 
progressivement en mode `reject` avec également un pourcentage graduel (voir cette 
[page](https://support.google.com/a/answer/2466563?hl=en) pour plus d'informations).

Pensez à bien vérifier au fur et à mesure que vos emails sont bien reçus et valides (par exemple Gmail indique 
clairement si l'expéditeur est reconnu ou non).

[SPF]: https://fr.wikipedia.org/wiki/Sender_Policy_Framework
[Runbox]: https://runbox.com/
[DKIM]: https://fr.wikipedia.org/wiki/DomainKeys_Identified_Mail
[DMARC]: https://fr.wikipedia.org/wiki/DMARC
