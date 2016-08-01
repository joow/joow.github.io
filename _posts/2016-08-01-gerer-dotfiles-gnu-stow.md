---
layout: post
title: Gérer ses dotfiles avec GNU Stow
---

De nombreux logiciels enregistrent leurs préférences dans des fichiers textes, en tout cas sur les systèmes 
d'exploitation de type UNIX/Linux, les logiciels Windows utilisant plutôt la base de registre.  
Ces fichiers sont généralement sauvegardés dans le répertoire personnel de l'utilisateur mais avec un nom préfixé par 
le caractère '.' afin qu'ils n'apparaissent pas par défaut dans le gestionnaire de fichiers et restent masqués. On 
parle alors de [dotfiles].

Dans le cas où l'on utilise les mêmes logiciels sur plusieurs machines différentes, avoir à reconfigurer 
systématiquement chaque poste devient vite fastidieux.  
Il existe plusieurs manières de gérer ses fichiers de configuration afin de permettre leur partage, celle que je 
présente dans ce billet se base sur le logiciel [GNU Stow] qui permet de gérer l'installation de logiciels à l'aide de 
liens symboliques. L'utiliser pour gérer ses fichiers de configuration n'est pas sa fonction première mais il s'y 
prête très bien.

## Mise en place

La première chose à faire est de créer un répertoire où seront stockés tous les fichiers de configuration, par exemple 
`~/dotfiles`. Ce répertoire contient un sous-répertoire par logiciel dont on souhaite partager la configuration. Chacun 
de ses sous-répertoires contient le ou les fichiers de configuration, en conservant l'arborescence nécessaire.   
Par exemple dans mon cas je souhaite partager ma configuration git et maven :

    dotfiles/
        git/
            .gitconfig
        maven/
            .m2/
                settings-security.xml
                settings.xml

Comme vous pouvez le voir chaque sous-répertoire conserve l'arborescence nécessaire aux fichiers de configuration. Afin 
“d'installer” la configuration de git, il me suffit d'utiliser la commande `stow git` et [stow] va automatiquement 
créer un lien symbolique dans mon répertoire personnel nommé `.gitconfig` qui pointera vers le fichier de mon 
répertoire `dotfiles/git`.

Il ne reste plus qu'à versionner (avec Git !) mes fichiers de configuration et les partager, par exemple avec [GitHub].  
**Attention** par contre à vos fichiers de configuration contenant des informations sensibles (clé de sécurité, token), 
vérifier bien que vous ne les partagez pas !

[dotfiles]: https://dotfiles.github.io/
[GNU Stow]: https://www.gnu.org/software/stow/
[stow]: https://www.gnu.org/software/stow/
[GitHub]: https://github.com