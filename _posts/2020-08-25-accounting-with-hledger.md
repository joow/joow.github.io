---
title: Gérer ses comptes avec hledger
layout: post
---

[hledger](https://hledger.org) est un outil de comptabilité en partie double.

Le principe de base est que toute transaction est décomposée entre plusieurs comptes, ceux débités et ceux crédités, le 
total des opérations étant toujours égal à 0.  
L'article [Wikipedia](https://fr.wikipedia.org/wiki/Comptabilit%C3%A9_en_partie_double) donne plus d'informations sur 
le sujet.  
La documentation de hledger présente également les bases de ce système, en anglais.

Pour débuter avec hledger il suffit d'installer le paquet pour votre système (disponible sur Windows, macOS, Linux...).

Toutes les transactions sont décrites dans un fichier au format texte, ce qui a l'avantage de vous laisser la main sur ce fichier en conservant un format ouvert.

Par défaut hledger utilise le fichier `.hledger.journal` situé à la racine de votre répertoire utilisateur.  
Néanmoins il est vivement recommandé de créer un dossier spécifique et un fichier par année puis d'exporter la variable d'environnement `HLEDGER_FILE` pour que la commande hledger utilise votre fichier par défaut.  
Il est également possible d'utiliser l'option `-f <CHEMIN>` ou `--file <CHEMIN>`. Vous pouvez par exemple utiliser un 
alias pour cela (ou une `abbr`éviation si vous utilisez comme moi shell fish).

De mon côté utilisant le shell [fish](https://fishshell.com/) j'ai utilisé la commande `set -Ux LEDGER_FILE <chemin>` 
pour déclarer cette variable d'environnement :
{% highlight shell %}
mkdir -p ~/Documents/Finance
touch ~/Documents/Finance/2020.journal
set -Ux LEDGER_FILE ~/Documents/Finance/2020.journal
{% endhighlight %}

Une fois ceci fait on peut ouvrir ce fichier et configurer l'affichage de l'unité monétaire :
```
commodity 1 000,00 EUR
```

Cela permet d'indiquer que le séparateur des milliers est l'espace, le séparateur décimal est le point et enfin le 
symbole monétaire est l'euro.  
Il est bien entendu possible de déclarer d'autres produits (`commodity`) par la suite, par exemple d'autres monnaies ou 
des actions, ce que nous verrons dans un prochain billet.

_Note_ : j'évite en général d'utiliser trop de termes étrangers mais la majorité des ressources disponibles sur les 
outils de comptabilité en partie double et le manuel de hledger étant en anglais je privilégierai parfois la langue 
anglaise afin de ne pas trop s'éloigner des termes originaux.

Il est ensuite temps de réfléchir à la façon dont nos comptes seront organisés.

Dans hledger tout est compte, mais vous pouvez plutôt penser parfois au terme catégorie qui est équivalent.  
Par exemple si je fais des courses je vais débiter 50 EUR de mon compte en banque (qui pour le coup est bien un compte) 
et créditer 50 EUR sur le compte courses, que l'on associerai habituellement plutôt à une catégorie.

hledger ne préconise aucune organisation particulière quant à vos comptes.  
Il vous facilite seulement la vie si vous les préfixez d'un mot dénotant le type du compte, qui sont au nombre de cinq :
* assets : ce que vous possédez (_actif_ en français)
* liabilities : ce que vous devez (_passif_)
* equity : montant investi (_capital_ mais ce type de comptes n'apparaît pas en comptabilité française)
* revenue : vos revenus
* expenses : vos dépenses

La base de la comptabilité en partie double est l'équation suivante :
```
assets = liabilities + equity
```

Cela revient à dire que ce que vous possédez vient soit de votre capital, soit de votre dette.

Un exemple d'organisation des comptes serait le suivant :
```
assets
  compte courant
  livret A
liabilities
  emprunt conso
  emprunt immo
equity
revenue
  travail
  cadeaux
expenses
  courses
  loyer
  électricité
```

Vos différents comptes en banque sont des `assets` (choses que vous possédez), votre prêt consommation ou immobilier 
sont des `liabilities` (choses que vous devez), les salaires versés par votre employeur ou les cadeaux reçus (comme un 
chèque à Noël) des revenus et enfin vos dépenses comme votre loyer, vos courses, votre facture d'électricité...

De mon côté comme je compte à terme utiliser hledger pour suivre un budget et mes investissements financiers je suis 
parti sur cette organisation :
```
assets
  budget
    disponible
      compte courant
      livret A
    courses
    loyer
    électricité
    ...
  épargne
    pel
  investissements
    pea
revenue
  travail
  cadeaux
expenses
  courses
  loyer
  électricité
```

Lorsque nous aborderons la gestion d'un budget cette organisation fera sens.

Notez que hledger permet de déclarer de nouveaux comptes à la volée lors de leur première utilisation dans une 
transaction.  
Il est également tout à fait possible de renommer et de réorganiser à postériori les comptes, c'est un des avantages du 
format ouvert utilisé.

Pour terminer ce billet nous allons initialiser les balances de nos comptes.

hledger propose la commande `add` pour ajouter de manière interactive une nouvelle transaction.  
Il est également possible d'éditer directement le journal, ce qui est en général ce que je fais car cela est plus 
rapide.

```
commodity 1 000,00 EUR

2020-08-24 * solde initial
    assets:budget:disponible:compte courant   500,00 EUR
    assets:budget:disponible:livret A       1 500,00 EUR
    assets:épargne:pel                      2 000,00 EUR
    equity:soldes d'ouverture
```

Une transaction est composée d'au moins 3 lignes :

  1. la première ligne contient la date de la transaction, son statut éventuel (`*` signifie que la transaction est 
  validée, cela nous permettra par la suite de réconcilier notre journal avec les relevés de nos comptes)
  2. les autres lignes contiennent les affectations (`posting` en anglais), avec le nom du compte affecté et le montant.
  Un nombre positif signifie un crédit et un nombre négatif un débit.

Comme vous pouvez le remarquer la dernière ligne ne mentionne pas le montant.  
hledger va automatiquement le déduire des autres lignes, il sera donc égal à `-4 000,00 EUR`.

Une fois notre journal sauvegardé nous pouvons utiliser les deux premières commandes de hledger (trois si vous avez 
utilisez la commande `add` pour initialiser le solde de vos comptes).

La commande `balance` (ou `bal` voire `b`) permet d'afficher la balance de tous vos comptes :
```
❯ hledger bal
        4 000,00 EUR  assets
        2 000,00 EUR    budget:disponible
          500,00 EUR      compte courant
        1 500,00 EUR      livret A
        2 000,00 EUR    épargne:pel
       -4 000,00 EUR  equity:soldes d'ouverture
--------------------
                   0
```

La commande `balancesheet` ou `bs` permet elle d'afficher la balance de vos comptes de type `assets` et `liabilities` :
```
❯ hledger bs
Balance Sheet 2020-08-24

                      ||   2020-08-24
======================++==============
 Assets               ||
----------------------++--------------
 assets               || 4 000,00 EUR
   budget             || 2 000,00 EUR
     disponible       || 2 000,00 EUR
       compte courant ||   500,00 EUR
       livret A       || 1 500,00 EUR
   épargne            || 2 000,00 EUR
     pel              || 2 000,00 EUR
----------------------++--------------
                      || 4 000,00 EUR
======================++==============
 Liabilities          ||
----------------------++--------------
----------------------++--------------
                      ||
======================++==============
 Net:                 || 4 000,00 EUR
```

Ces deux commandes possèdent bien entendu de nombreuses options mais pour le moment nous nous arrêterons là.

Dans le prochain billet sur hledger nous verrons comment enregistrer nos transactions quotidiennes.
