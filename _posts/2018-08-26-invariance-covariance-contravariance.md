---
title: Invariance, covariance et contravariance
layout: post
---

En programmation orientée objet il existe la notion d'invariance, covariance et contravariance.

Que signifie ces termes exactement ?

Si `A` et `B` sont deux types, que `A` est un sous-type de `B` et `f` une transformation de type alors :

  - `f` est covariant si `f(A)` est un sous-type de `f(B)`
  - `f` est contravariant si `f(B)` est sous-type de `f(A)`
  - `f` est invariant si aucune des deux propositions précédentes est vraie

_Il existe également la notion de bivariance, où les deux propositions sont vraies._

En Java si on considère les types génériques comme une transformation, on peut alors dire qu'ils sont invariants.  
En effet `String` est un sous-type de `Object` mais `List<String>` n'est pas un sous-type de `List<Object>` (pas plus 
que `List<Object>` n'est un sous-type de `List<String>`).

Les tableaux par contre sont covariants. En effet `String[]` est un sous-type de `Object[]`.

Il n'y a pas de fonctions contravariantes en Java, contrairement à Scala par exemple.
