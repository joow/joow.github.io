---
title: Configuration d'IntelliJ IDEA
layout: post
---

L'environnement de développement [IntelliJ IDEA](https://www.jetbrains.com/idea/) propose une configuration par défaut plutôt adaptée aux standards du marché. Il est d'ailleurs en général de bon ton de rester le plus proche possible des configurations par défaut afin de ne pas passer de temps à refaire sa configuration lors de la réinstallation d'une machine.

Il existe néanmoins quelques paramètres qu'il me semble intéressant de modifier :

- Editor > General > Auto Import :  
  Pour le langage `Java` :
    - Activer `Optimize imports on the fly (for current project)`
    - Activer `Add unambiguous imports on the fly`
- Editor > General > Appearance :  
  Configurer `Show parameter name hints` :
    - Sélectionner le langage `Kotlin` et activer :  
      - `Show property type hints`
      - `Show local variable type hints`
      - `Show function return type hints`
      - `Show argument name hints`
      - `Show lambda return expression hints`
      - `Show hints for implicit receivers and parameters of lambdas`
- Editor > Code Style> Java :
  - Paramétrer `Class count to use import with '*'` à `999`
  - Paramétrer `Names count to use static import with '*'` to `999`
- Keymap :  
  Changer éventuellement les raccourcis clavier des actions `Comment with Line Comment` et `Comment with Block Comment`

_Note_ : le raccourci clavier pour changer de fenêtre en vue fractionnée est normalement `Alt+Tab`
