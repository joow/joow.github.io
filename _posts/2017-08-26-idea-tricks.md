---
layout: post
title: Utiliser l'outil de comparaison de fichiers d'IntelliJ IDEA
---

[IntelliJ IDEA] (ainsi que les autre produits de JetBrains) propose un outil de comparaison de fichiers très puissant.

Si vous souhaitez l'utiliser également en dehors d'[IntelliJ IDEA] il faut tout d'abord créer le lanceur en ligne de commande qui permet de démarrer [IntelliJ IDEA] depuis le terminal. Pour cela allez dans le menu `Tools>Create Command-line launcher...`  
Une fois ceci fait la commande `idea` devrait être disponible en ligne de commande.

Pour comparer deux fichiers utilisez simplement la commande suivante :

    idea diff <fichier1> <fichier2>

Ou cette commande pour comparer deux répertoires :

    idea diff <répertoire1> <répertoire2>

[IntelliJ IDEA]: https://www.jetbrains.com/idea/
