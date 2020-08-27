---
title: Réconcilier son journal hledger
layout: post
---

En utilisant hledger pour suivre vos finances vous souhaiterez réconcilier votre journal avec le relevé de vos comptes 
bancaires afin de vérifier que vous avez bien enregistré toutes vos transactions.

Pour effectuer cette opération vous allez récupérer votre relevé de compte, aujourd'hui plus souvent en consultant 
simplement le site de votre banque ou l'application disponible sur votre téléphone, pour pointer les opérations 
apparaissant sur votre journal et sur votre relevé.

Pour commencer à vérifier si votre journal est correctement réconcilié il vous suffit d'utiliser la commande 
`hledger bs --cleared`, qui ne prendra en compte que les transactions marquées comme traitée.  
Ces transactions ont comme entête dans leur titre le caractère `*`.  
Vous pouvez également ajouter le nom de votre compte pour n'afficher que la balance de celui-ci (utile dès que vous 
avez de nombreux comptes de type `assets` et `liabilities`), hledger fonctionne par motif et affichera tous les comptes 
correspondants, par exemple `hledger bs --cleared 'livret A'` pour afficher uniquement la balance de votre livret A 
(notez les apostrophes, le nom du compte contenant un espace).

Si le montant affiché correspond à celui de votre banque vous n'avez rien à faire.  
Si par contre le montant est différent il vous suffit de pointer les opérations qui sont bien inscrites sur votre 
compte bancaire, et ce jusqu'à ce que les deux montants correspondent.

Personnellement j'essaie de faire cette opération au moins tous les quinze jours afin que cela ne soit pas trop long.  
Suivant le nombre d'opérations que vous faites quotidiennement il vaut mieux faire cette opération plus ou moins 
souvent afin que cela ne vous prenne pas trop de temps.
