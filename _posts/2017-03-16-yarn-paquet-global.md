---
layout: post
title: Yarn et les paquets globaux
---

[Yarn] est un gestionnaire de paquets JavaScript assez récent mais qui a déjà beaucoup de succès.  
Et pour cause ! Il est plus rapide que [npm] et permet de verrouiller très finement les versions des paquets 
installés.  

Par contre attention, si vous l'avez installé par le biais de votre gestionnaire de paquets habituels (par exemple 
[brew] alors que vous utilisez [nvm] pour gérer vos versions de [node], les paquets installés 
globalement par [Yarn] ne seront pas installés avec la version locale de [node] et risquent de ne pas être trouvés.  

Pour palier à ce problème vous pouvez soit :

  - installer [Yarn] comme paquet global de votre version de [node] gérée par [nvm]
  - utiliser exceptionnellement [npm] pour installer les paquets globalement ([npm] étant lui installée avec la version 
    de [node])

De toute façon installer des paquets globalement est déconseillé :smile:

[Yarn]: https://yarnpkg.com
[npm]: https://www.npmjs.com/
[brew]: https://brew.sh/
[nvm]: https://github.com/creationix/nvm
[node]: https://nodejs.org