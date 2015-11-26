---
layout: post
title: Utiliser une variable Maven dans un script Groovy
---
Le plugin [GMaven] permet d'enrichir un script de construction [Maven] en utilisant du code [Groovy].  
[Maven] possède un mécanisme permettant de définir des variables qui seront remplacées par leur valeur effective lors de l'exécution de la construction.  
Il est bien sûr possible d'utiliser ces variables dans les scripts [Groovy]. L'un des problèmes qui peut alors se poser est que la construction échouera sur une machine Windows si l'une des variables représente un chemin. En effet le chemin contiendra des caractères '\\' qui ne sont pas autorisées dans des chaînes de caractères (*il faut les échapper avec ce même caractère*).

Pour pallier à cela il faut utiliser la variable `project.properties` qui est un dictionnaire (*map*) contenant toutes les propriétés [Maven].
Ainsi si vous voulez utiliser une propriété nommée `filePath` il faut remplacer `${filePath}` par `${project.properties['filePath']}`.
La dernière solution permet d'éviter que [Maven] remplace la propriété par la valeur, le script Groovy accèdant à la valeur de la propriété à l'exécution, évitant toute erreur si la propriété contient des caractères invalides.

[GMaven]: http://groovy.github.io/gmaven/
[Maven]: http://maven.apache.org/
[Groovy]: http://www.groovy-lang.org/