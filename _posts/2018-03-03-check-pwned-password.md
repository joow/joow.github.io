---
layout: post
title: Vérifier si son compte a été compromis
---

Le site ["Have I been pwned?"](https://haveibeenpwned.com) aggrége la liste des mots de passe divulgués suite à des fuites de données.
Il propose également une API permettant d'interroger cette énorme base de données (presque de 5 milliards de comptes provenant de 270 sites).

L'une des fonctionnalités proposées est la recherche par début d'empreinte, décrite [ici](https://haveibeenpwned.com/API/v2#SearchingPwnedPasswordsByRange).  
Il est ainsi possible de vérifier une fuite d'un mot de passe en fournissant uniquement les cinq premiers caractères de son empreinte [SHA-1](https://fr.wikipedia.org/wiki/SHA-1), évitant ainsi de transmettre le mot de passe.

Sur un système UNIX vous pouvez utiliser les commandes suivantes :

     echo -n "<PASSWORD>" | sha1sum (ne pas oublier de mettre un espace pour ne pas enregistrer la commande dans l'historique du shell)
    http https://api.pwnedpasswords.com/range/<BEGIN OF HASH> (les cinq premières lettres de l'empreinte SHA-1)

La liste des empreintes correspondantes sera renvoyée et il ne restera plus qu'à vérifier si oui ou non l'empreinte complète du mot de passe à vérifier est dans cette liste.

Cette fonctionnalité pourrait être également intégrée dans un site ou une application afin d'interdire les mots de passe trop communs, l'API renvoyant également avec chaque empreinte son nombre d'occurences.
