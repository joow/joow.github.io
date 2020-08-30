---
title: Gérer son budget avec hledger
layout: post
---

> Note : je n'en n'ai jamais parlé jusqu'à présent mais le journal hledger étant un simple fichier n'hésitez pas à créer un dépôt Git afin de pouvoir valider régulièrement vos modifications, réorganiser vos comptes et tester votre budget en gardant toujours des versions historisées de votre journal.  
> Sauvegardez bien ce dépôt Git, mais pas sur GitHub, il contient toutes vos données financières !

## Une histoire de courrier

Jusqu'à présent nous avons vu comment utiliser hledger afin de suivre la balance de nos comptes en inscrivant au fur et à mesure les différentes transactions et en validant régulièrement que le solde de notre journal correspondait à celui du relevé de nos comptes en banque.

hledger peut également être utilisé pour suivre son budget.  
Cette fonctionnalité se base sur l'utilisation des [transactions périodiques](https://hledger.org/journal.html#periodic-transactions).

Bien que cette fonctionnalité soit intéressante elle permet seulement de visualiser la balance prévisionnelle en fonction de ces transactions périodiques.  
Il également possible de voir après coup l'utilisation réelle vis à vis de notre budget prévisionnel, mais cela permet juste de constater un dépassement de notre budget après coup.

Cette solution peut tout à fait vous convenir mais je suis personnellement adepte de la méthode des enveloppes pour gérer mon budget.  
Pour résumer la méthode il s'agit de répartir tout argent reçu entre nos différents postes de dépenses et de les consommer au fur et à mesure.  
S'il l'on dépasse le budget prévu pour une catégorie il faudra prendre l'argent d'une autre catégorie.  
Cette méthode peut être visualisée facilement en imaginant que chaque fois que vous recevez votre salaire vous le récupérez sous forme d'argent liquide et que vous répartissiez cet argent liquide dans différentes enveloppes portant le nom de leur catégorie, par exemple _courses_, _essence_, _sorties_, _loyer_, _électricité_...  
Lorsque vous allez faire des courses vous prenez une partie de l'argent disponible dans l'enveloppe _courses_.  
Si vous avez dépensé tout l'argent de cette enveloppe il faudra prendre de l'argent d'une autre enveloppe pour pouvoir faire les courses.  
Les catégories sont bien entendu libres et dépendent entièrement de la façon et de la granularité avec laquelle vous voulez suivre votre budget.  
Personnellement je n'ai par exemple qu'une catégorie "courses" mais vous pourriez très bien avoir une catégorie _nourriture_, une autre _produits de soin_, encore une autre _produits d'entretien_ et ainsi de suite dans le cas où vous souhaitez suivre plus précisément vos dépenses.  
Je ne recommande pas forcément d'aller trop loin afin de ne pas passer trop de temps à décortiquer ses comptes mais à chacun d'adapter le système selon ses préférences.

En suivant régulièrement le montant restant dans nos enveloppes respectives on peut ajuster notre consommation.

Si vous n'avez jamais utilisé cette technique je vous conseille de l'expérimenter, le simple fait d'établir la liste de vos enveloppes vous fera prendre conscience de vos différents postes de dépense et vous incitera peut-être à être plus économe sur certaines catégories ou au contraire mettre de l'argent de côté pour d'autres qui vous tiennent à coeur.

## Mise en place sur hledger

Il est tout à fait possible de mettre en place un mécanisme similaire avec hledger.

Si vous vous rappelez bien lorsque nous avons initialisé les soldes de nos comptes j'ai mis les comptes bancaires sous la catégorie "assets:budget:disponible".  
Si je demande à hledger la balance disponible avec la commande `hledger bal dispo` (qui ne devrait correspondre qu'au compte _disponible_ et ses sous-comptes) je vais obtenir le montant que je peux allouer à mes différentes enveloppes.

Il ne me "reste" plus qu'à allouer ce montant disponible aux enveloppes que je souhaite dans une transaction me permettant d'initialiser mon budget.  
Je dit "reste" car cela peut prendre un peu de temps, en particulier la première fois si vous débutez et que vous devez déjà définir vos catégories, et également car il manque un élément important pour affecter l'argent de nos comptes en banque utilisés pour le budget à nos différentes enveloppes.  
Si jamais j'utilise des affectations normales cela revient à dire qu'une partie de l'argent de mon compte courant va passer sur mon compte "budget:courses", alors que ce ne sera pas réellement le cas, l'argent reste bien sur mon compte courant tant que je ne l'ai pas réellement dépensé.  
De plus si je souhaite réconcilier mon compte courant avec mon relevé bancaire les montants ne correspondront plus du tout, ce qui est plutôt gênant !

La solution pour gérer nos enveloppes est d'utiliser des [affectations virtuelles](https://hledger.org/journal.html#virtual-postings).  
Ces affectations sont similaires aux affectations normales mais le nom du compte est mis entre crochets (il est également possible de le mettre entre parenthèse mais cela a pour effet de permettre d'ignorer la règle selon laquelle toutes les affectations d'une transaction doit avoir une somme égale à 0, comme la documentation l'indique il est déconseillé d'utiliser ce type d'affectation).  
L'avantage des affectations virtuelles est qu'il est ensuite possible de les exclure des différents rapports en utilisant l'option `--real`, ce qui nous permettra d'afficher le solde réel de notre compte.

Si vous avez par exemple 1 950 euros à affecter à votre budget votre transaction d'initialisation du budget ressemblerait à ça :
```
2020-08-30 budget
    [assets:budget:courses]                        200,00 EUR
    [assets:budget:loyer]                          800,00 EUR
    [assets:budget:loisirs]                        100,00 EUR
    [assets:budget:électricité]                     50,00 EUR
    [assets:budget:vacances]                       800,00 EUR
    [assets:budget:disponible:livret A]          -1500,00 EUR
    [assets:budget:disponible:compte courant]     -450,00 EUR
```

Si vous affichez désormais le montant disponible vous verrez qu'il est passé à 0 :
{% highlight shell %}
❯ hledger bal dispo
--------------------
                   0
{% endhighlight %}

Si par contre vous affichez votre budget vous pouvez voir les montants disponibles dans chacune de vos enveloppes :
{% highlight shell %}
❯ hledger bal budget
        1 950,00 EUR  assets:budget
          200,00 EUR    courses
          100,00 EUR    loisirs
          800,00 EUR    loyer
          800,00 EUR    vacances
           50,00 EUR    électricité
--------------------
        1 950,00 EUR
{% endhighlight %}

Enfin si vous voulez voir le montant réel de vos comptes en banque vous pouvez ajouter l'option `--real` :
{% highlight shell %}
❯ hledger bs --real
Balance Sheet 2020-08-30

                      ||   2020-08-30
======================++==============
 Assets               ||
----------------------++--------------
 assets               || 3 950,00 EUR
   budget             || 1 950,00 EUR
     disponible       || 1 950,00 EUR
       compte courant ||   450,00 EUR
       livret A       || 1 500,00 EUR
   épargne            || 2 000,00 EUR
     pel              || 2 000,00 EUR
----------------------++--------------
                      || 3 950,00 EUR
======================++==============
 Liabilities          ||
----------------------++--------------
----------------------++--------------
                      ||
======================++==============
 Net:                 || 3 950,00 EUR
{% endhighlight %}

## Retour au monde réel

Il manque une dernière pièce à notre puzzle pour que la mise en place de nos enveloppes dans hledger soient complète.

Lorsque je vais faire une dépense réelle d'une de mes enveloppes je vais prendre l'argent sur mon compte bancaire.  
Mais avec notre mécanisme le montant de la dépense ne sera pas retirée de l'enveloppe, et par conséquent je ne verrais 
pas le montant disponible dans mon enveloppe diminuer, annihilant tout l'intérêt de la mise en place du système.  
Je pourrais plutôt prendre l'argent déposé dans l'enveloppe mais cette fois c'est mon compte bancaire qui ne serait pas 
débité !

La solution est de rajouter deux affectations virtuelles dans notre dépense, transférant l'argent de notre enveloppe vers notre compte en banque afin de pouvoir le dépenser :
```
2020-08-31 courses
    [assets:budget:disponible:compte courant]       50,00 EUR
    [assets:budget:courses]                        -50,00 EUR
    expenses:courses                                50,00 EUR
    assets:budget:disponible:compte courant        -50,00 EUR
```

Facile non ?  
Le gros inconvénient de cette solution est que l'on doit systématiquement ajouter deux affectations à chaque 
transaction de notre budget.  
Heureusement hledger propose une solution avec les [affectations automatiques](https://hledger.org/journal.html#auto-postings).  
Ce type d'affectation est généré automatiquement en fonction d'une requête :
```
= expenses:courses
    [assets:budget:courses]                     *-1
    [assets:budget:disponible:compte courant]   *1
```

La première ligne indique la requête pour sélectionner les transactions correspondante.  
Ici si la transaction contient une affectation sur le compte "expenses:courses" deux affectations sont ajoutées, 
décrite dans les deux lignes suivantes :
  1. la première sur le compte "assets:budget:courses" d'un montant égal au montant des courses multiplié par -1 (si 
  j'ai dépensé 50 euros une affectation débitant 50 euros sera créée sur mon enveloppe course)
  2. la seconde sur le compte courant, d'un montant égal au montant des courses multiplié par 1 (donc égal), ce qui permet de créditer mon compte courant juste avant de les dépenser

Il vous suffit ensuite de rajouter l'option `--auto` pour afficher votre budget :
{% highlight shell %}
❯ hledger bs budget --auto
Balance Sheet 2020-08-31

                 ||   2020-08-31
=================++==============
 Assets          ||
-----------------++--------------
 assets          || 1 900,00 EUR
   budget        || 1 900,00 EUR
     courses     ||   150,00 EUR
     loisirs     ||   100,00 EUR
     loyer       ||   800,00 EUR
     vacances    ||   800,00 EUR
     électricité ||    50,00 EUR
-----------------++--------------
                 || 1 900,00 EUR
=================++==============
 Liabilities     ||
-----------------++--------------
-----------------++--------------
                 ||
=================++==============
 Net:            || 1 900,00 EUR
{% endhighlight %}

**Attention** : les affectations automatiques vont s'appliquer à toutes les transactions, donc il est recommandé de mettre en place ce mécanisme dès le début ou dans un nouveau fichier afin de ne pas fausser les transactions faites avant la mise en place des enveloppes.  
Il est également possible de créer une transaction rétroactivement qui va initialiser un budget pour les transactions 
précédentes.

## Bonus

Pour finir ce billet déjà assez long une petite astuce permettant de simplifier l'utilisation du budget et plus 
généralement hledger.  
Afin d'éviter d'avoir à écrire systématiquement le nom complet d'un compte comme 
"assets:budget:disponible:compte courant" hledger offre la possibilité de définir des 
[alias](https://hledger.org/journal.html#rewriting-accounts) afin de raccourcir le nom des comptes à saisir dans vos 
transactions.  
Il vous suffit de rajouter cette instruction dans votre journal :
```
alias compte courant = assets:budget:disponible:compte courant
```

Désormais vous pouvez utiliser directement le nom "compte courant" dans vos affectations, hledger le remplacera 
automatiquement par le nom complet :-)

Ce billet conclut notre découverte de l'outil hledger.  
Il reste encore à aborder le suivi des emprunts et des comptes d'investissements (assurance vie, plan épargne actions...).  
Une fois que j'aurais moi-même mis en place ce suivi dans mon journal je ne manquerai pas de consacrer quelques billets pour vous partager l'utilisation de hledger dans ce contexte.
