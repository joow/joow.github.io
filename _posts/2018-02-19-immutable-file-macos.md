---
layout: post
title: Rendre un fichier immuable sous macOS
---
Sous les systèmes GNU/Linux il est possible de rendre un fichier immuable en utilisant la commande `chattr`. Cela permet ainsi de s'assurer que l'on ne supprimera pas un fichier par inadvertance.

La commande équivalente sur macOS (et sur les systèmes BSD en général) est `chflags` qui permet de manipuler les flags d'un fichier.  
Ainsi on peut rendre un fichier immuable pour l'utilisateur en utilisant le flag `uimmutable` (`uchg` ou `uchange` existent également) :

    # touch keep.txt
    # chflags uimmutable keep.txt
    # rm keep.txt
    override rw-r--r--  benoit/staff uchg for keep.txt? y
    rm: keep.txt: Operation not permitted

L'utilisateur `root` peut lui bien entendu supprimer le fichier :

    sudo rm keep.txt
    Password:
    override rw-r--r--  benoit/staff uchg for keep.txt? y

Pour faire en sorte que même l'utilisateur `root` ne puisse pas supprimer le fichier il faut utiliser le flag `simmutable` (ou bien `schg` ou encore `simmutable`). Bien entendu seul l'utilisateur `root` peut activer ce flag.

Pour désactiver un flag il faut ajouter `no` devant le flag : `noimmutable` ou `nosimmutable` par exemple.

Afin de voir les flags d'un fichier il est nécessaire d'ajouter l'argument `-O` à la commande `ls` en utilisant l'affichage long `-l` : `ls -lO`
