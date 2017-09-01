---
layout: post
title: La décomposition en JavaScript
---

Cet article va brièvement présenter l'affectation par décomposition en JavaScript (*destructuring* en anglais), 
introduite avec ECMAScript 2015 pour les tableaux et pour l'instant en [stage 3](https://github.com/tc39/proposal-object-rest-spread) pour les objets.  
En quelques mots, cette opération offre la possibilité d'extraire des données d'un tableau ou d'un objet de manière 
très simple.  

Afin de présenter la décomposition, nous allons utiliser [Jest](https://facebook.github.io/jest/), un 
outil de test très simple et malgré tout très puissant initié par Facebook.

## Initialiser le projet

Commençons par créer un répertoire pour notre projet :
    
    mkdir destructuring
    cd destructuring

Puis initialisons un nouveau projet en gardant les options par défaut, à l'aide de [Yarn](https://yarnpkg.com)
(ou npm si vous préfèrez) :

    yarn init --yes

Une fois ceci fait il suffit d'ajouter la librairie Jest :

    yarn add jest --dev

Enfin ajoutons un script à notre fichier `package.json` pour lancer les tests à l'aide de Jest :

    "scripts": {
      "test": "jest"
    }

## Décomposition de tableaux

Pour décomposer un tableau lors d'une affectation le même symbole que celui d'un tableau va être utilisé : `[]`.

On va créer un premier fichier qui contiendra tous nos tests relatifs à la décomposition de tableaux : 
`array-destructuring.test.js`  
La syntaxe générale d'un test avec Jest est la suivante :

    test('description', => {
      expect(...).toBe(...);
    });
  
### Récupérer le premier élément d'un tableau

    test("récupérer le premier élément d'un tableau", () => {
      const arr = [1, 2, 3];
      const [head] = arr;
      expect(head).toBe(1);
    });

Lancer la commande `yarn test` (ou `npm test`) afin d'exécuter le test.

### Récupérer les deux premiers éléments d'un tableau

Pour ce test on va à nouveau utiliser le même tableau de trois nombres que pour le test précédent, on peut donc le 
rendre global à notre script afin de le partager dans tous les tests :

    const arr = [1, 2, 3];

    test("récupérer les deux premiers éléments d'un tableau", () => {
      const [first, second] = arr;
      expect(second).toBe(2);
    });

Je pense que vous commencez à comprendre la syntaxe générale :wink:.

### Récupérer un élément d'un tableau inexistant

Je pourrais vous montrer le test permettant de récupérer trois éléments d'un tableau mais on va plutôt voir désormais 
ce qu'il se passe si on essaie de récupérer un élément inexistant :

    test("récupérer un élément inexistant d'un tableau", () => {
      const [first, second, third, fourth] = arr;
      expect(fourth).toBeUndefined();
    });

Comme on s'en rend compte dans le test, notre élément n'existant pas dans le tableau on reçoit la valeur `undefined`.

### Récupérer un élément d'un tableau inexistant avec une valeur par défaut

Dans le cas où l'on sait que l'on va peut-être récupérer un élément inexistant (et donc indéfini), il est possible 
d'indiquer une valeur par défaut :

    test("récupérer un élément inexistant avec une valeur par défaut", () => {
      const [first, second, third, fourth = 4] = arr;
      expect(fourth).toBe(4);
    });

### Passer un élément

Finalement on souhaite récupérer le troisième élément, mais sans récupérer cette fois le second :

    test("passer un élément", () => {
      const [first,, third] = arr;
      expect(third).toBe(3);
    });

## Récupérer la queue d'un tableau

Grâce à l'opération de décomposition `...` on peut également affecter le reste de notre tableau à une variable :

    test("récupérer la queue d'un tableau", () => {
      const [head, ...tail] = arr;
      expect(tail).toEqual([2, 3]);
    });

### Concaténer deux tableaux

L'opérateur de décomposition `...` est très utile afin de simplifier l'utilisation des fonctions attendant une liste 
d'éléments :

    test("concaténer deux tableaux", () => {
      const first = [1, 2, 3];
      const second = [4, 5, 6];
      first.push(...second);
      expect(first).toEqual([1, 2, 3, 4, 5, 6]);
    });

### Trouver le maximum d'un tableau de nombres

Cette syntaxe permet de simplifier par exemple l'appel de la fonction `Math.max()` qui attend une liste de nombres :

    test("trouver le maximum d'un tableau de nombres", () => {
      const numbers = [12, 42, 5, 33, 21];
      const max = Math.max(...numbers);
      expect(max).toBe(42);
    });

Auparavant il fallait se rappeler d'utiliser la fonction `apply` sinon on obtenait une erreur :

    Math.max(numbers); // KO => NaN
    Math.max.apply(null, numbers); // OK

## Décomposition d'objets

Un peu à la manière des tableaux la décomposition d'objets va elle utiliser le symbole `{}` également utilisé
pour créer des objets de manière littérale.

### Extraire des données d'un objet

On va créer un fichier `object-destructuring.test.js` afin d'écrire nos tests relatifs à la décomposition d'objets.

Pour commencer nous allons créer l'objet que nous allons manipuler :

    const person = {
      name: "SpongeBob",
      age: 31,
      address: {
        number: 124,
        street: "Conch Street",
        city: "Bikini Bottom",
      },
    };

On peut écrire notre premier test permettant d'extraire le nom et l'âge de notre personne :

    test("extraire des données d'un objet", () => {
      const {name, age} = person;
      expect(name).toBe("SpongeBob");
      expect(age).toBe(31);
    });

### Extraire des données imbriquées

Et si l'on souhaite extraire une donnée imbriquée dans un autre objet ? Pas de problème, il suffit d'imbriquer le 
symbole `{}` en fonction du chemin à atteindre pour récupérer notre donnée :

    test("extraire une donnée imbriquée", () => {
      const {address: {city}} = person;
      expect(city).toBe("Bikini Bottom");
    });

### Extraire une donnée inexistante

Si on essaie d'extraire une donnée inexistante, on aura comme pour la décompositionde tableaux la valeur `undefined` :

    test("extraire une donnée inexistante", () => {
      const {address: {country}} = person;
      expect(country).toBeUndefined();
    });

Afin de palier à cette éventuelle absence, on peut indiquer une valeur par défaut :

    test("extraire une donnée inexistante avec une valeur par défaut", () => {
      const {address: {country = "Pacific Ocean"}} = person;
      expect(country).toBe("Pacific Ocean");
    });

À noter que si l'on essaie de récupérer une donnée d'un objet imbriqué inexistant on aura bien une erreur à l'exécution :

    test("extraire une donnée inexistante d'un objet imbriqué inexistant", () => {
      const failure = () => {
        const {adress: {country}} = person; // adress doesn't exist, it is spelled address !
      }

      expect(failure).toThrow(TypeError);
    });

### Renommer une donnée

Il est possible de renommer une donnée extraite d'un objet en suffixant la donnée à extraire par `:<nouveau nom>`:

    test("renommer une donnée", () => {
      const {name:nom, age} = person;
      expect(nom).toBe("SpongeBob");
    });

### Décomposer les paramètres d'une fonction

Enfin grâce à l'opérateur de décomposition il est possible d'extraire les valeurs d'un objet passé en paramètre d'une fonction :

    test("décomposer les paramètres d'une fonction", () => {
      const summary = ({name, age}) => `${name} is ${age} years old.`;
      expect(summary(person)).toBe("SpongeBob is 31 years old.");
    });

Voilà, vous savez désormais presque tout sur la décomposition d'objets en JavaScript :smiley:

Vous pouvez retrouvez le code présenté sur le dépôt GitHub [suivant](https://github.com/joow/js-destructuring).