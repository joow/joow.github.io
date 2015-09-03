---
layout: post
title: Contribuer à un projet hébergé sur GitHub
---
Voici une procédure indiquant la manière recommandée de contribuer à un projet hébergé sur [GitHub]. Vous pouvez également vous référer à la méthode préconisée par [GitHub] en consultant cette [page].

1. Commencez par dupliquer le projet (_fork_). Ceci va vous permettre d'enregistrer vos modifications sur votre propre dépôt.
2. Clonez votre dépôt :  
``git clone https://github.com/<utilisateur>/<dépôt>``
3. Ajoutez le projet original comme référence distante, cela vous permettra de mettre à jour votre dépôt (``git pull <utilisateur original> master``) :  
``git remote add <utilisateur original> https://github.com/<utilisateur original>/<dépôt>``
4. Créez une branche thématique pour vos changements :  
``git checkout -b <branche>``
5. Effectuez et enregistrez vos modifications.
6. Poussez votre branche sur votre dépôt [GitHub] :  
``git push --set-upstream origin <branche>``
7. Enfin créez une pull request sur le projet original en utilisant comme source votre branche.

[GitHub]: https://github.com/
[page]: https://guides.github.com/activities/contributing-to-open-source/#contributing