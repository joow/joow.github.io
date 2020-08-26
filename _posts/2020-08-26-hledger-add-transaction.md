---
title: Enregistrer une transaction avec hledger
layout: post
---

Dans le précédent billet sur [hledger](https://hledger.org) nous avons vu comment initialiser nos différents comptes 
bancaires et leurs soldes respectifs.

Ce billet rapide va décrire la façon d'ajouter une nouvelle transaction notre journal, suite par exemple à une dépense.

## hledger add

La première possibilité pour ajouter une transaction est d'utiliser la commande `hledger add` qui offre un assistant 
interactif pour enregistrer notre transaction :

{% highlight shell %}
❯ hledger add
Date [2020-08-25]:
Description: Courses
Account 1: assets:budget:disponible:compte courant
Amount  1: -50 EUR
Account 2: expenses:courses
Amount  2 [50 EUR]:
Account 3 (or . or enter to finish this transaction): .
2020-08-25 Courses
    assets:budget:disponible:compte courant             -50
    expenses:courses                                     50

Save this transaction to the journal ? [y]: y
Saved.
Starting the next transaction (. or ctrl-D/ctrl-C to quit)
Date [2020-08-25]: .
{% endhighlight%}

La commande vous demande la date (par défaut celle du jour), la description de la transaction, le premier compte, que 
vous pouvez autocompléter en utilisant la touche `TAB`, le montant, puis le deuxième compte.  
Si votre transaction ne concerne que deux comptes vous pouvez laisser le second montant vide, il sera automatiquement 
déduit du premier (rappelez-vous que la somme de toutes les affectations d'une transaction est égale à 0).  
Vous n'avez plus qu'à valider pour que la transaction soit ajoutée au journal et quittez la commande comme indiquée.

## Éditeur de texte

Il est également possible d'ajouter la transaction directement dans le journal en utilisant votre éditeur de texte 
favori.  
Certains proposent même des plugins pour vous aider dans la saisie, comme par exemple 
[vim-ledger](https://github.com/ledger/vim-ledger).

## Autres moyens

Le projet hledger propose également les commandes `hledger-web` et `hledger-ui` qui permettent d'avoir respectivement 
une interface web et une interface dans un terminal pour gérer vos comptes et ajouter des transactions.

Par contre la commande `hledger-ui` n'est malheureusement pas disponible sur Windows (seulement dans WSL).
