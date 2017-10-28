---
layout: post
title: Extension et redéfinition en Kotlin
---

En Kotlin par défaut les classes ne sont pas extensibless et les méthodes ne sont pas redéfinissables, il faut autoriser explicitement la possiblité de les étendre / redéfinir en utilisant le mot clé `open` :

    open class Animal

    class Cat : Animal()

    open class Car {
        // drive peut être redéfinie
        open fun drive() {
            ...
        }

        // stop ne peut pas être redéfinie
        fun stop() {
            ...
        }
    }

    class Tesla : Car() {
        override fun drive() {
            ...
        }
    }

_Note_ : Le mot clé `override` est obligatoire en Kotlin, contrairement à Java. Ainsi si la méthode est supprimée dans la classe mère une erreur sera levèe à la compilation.

Par contre si une classe implémente une interface, les méthodes de l'interface seront redéfinies et ouvertes à la redéfinition par défaut.  
Dans la classe implémentant l'interface si on souhaite interdire la redéfinition de la méthode il faut utiliser le mot clé `final` (`override` implique `open`) :

    interface Drivable {
        fun drive()

        fun stop()
    }

    class Car : Drivable {
        // drive peut être redéfinie
        open fun drive() {
            ...
        }

        // stop ne peut pas être redéfinie
        final fun stop() {
            ...
        }
    }

    class Tesla : Car() {
        override fun drive() {
            ...
        }
    }

_Note_ : il est important de noter également que les interfaces sont obligatoirement ouvertes à l'extension, créer une interface non implémentable ou extensible n'aurait aucun intérêt.