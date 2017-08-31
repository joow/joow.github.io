---
layout: post
title: Téléchargements multiples avec Wget
---

[Wget](https://www.gnu.org/software/wget/) est un puissant outil de téléchargement en ligne de commandes.  
Il peut par exemple télécharger de multiples fichiers à partir d'un motif donné.  
Par exemple : 

    wget https://url/file{0..9}.tar.gz

va permettre de télécharger tous les fichiers nommés `file0.tar.gz`, `file1.tar.gz` jusqu'à `file9.tar.gz` en une seule commande :smiley:.
