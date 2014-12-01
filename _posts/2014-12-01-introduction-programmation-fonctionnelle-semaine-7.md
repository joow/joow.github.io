---
layout: post
title: Introduction à la programmation fonctionnelle - Semaine 7
---

Avant-dernier billet sur le [cours d'introduction à la programmation fonctionnelle].  
En effet cette semaine étaient proposées les dernières vidéos et la semaine prochaine comportera seulement des exercices. J'aurai l'occasion de revenir dessus et ainsi de conclure mon expérience et présenter mon ressenti.

## Évaluation paresseuse

La première partie de cette semaine était consacrée à l'évaluation paresseuse et les différentes manières d'évaluer une expression :

{% highlight haskell %}
square :: Num a => a -> a
square n = n * n

-- évaluation "la plus à l'intérieure" (innermost)
square (3 + 4) => square 7 => 7 * 7 => 49

-- évaluation "la plus à l'extérieure" (outermost)
square (3 + 4) => (3 + 4) * (3 + 4) => 7 * (3 + 4) => 7 * 7 => 49
{% endhighlight %}

Si une évaluation est terminale, l'évaluation _"la plus à l'extérieure"_ se terminera forcément alors que ce n'est pas le cas pour l'évaluation _"la plus à l'intérieure"_ :
{% highlight haskell %}
loop = head loop

fst(1, loop)
{% endhighlight %}

Dans ce cas on peut voir que `loop` est une expression à priori infinie. Dans le cas d'une évaluation _"la plus à l'intérieure"_ l'expression `loop` sera déjà évaluée et ne se terminera donc jamais. Par contre l'évaluation _"la plus à l'extérieure"_ renverra immédiatement 1 (la fonction `fst` renvoie la première valeur d'une paire).  
C'est pour cela que le langage Haskell est évalué de la manière _"la plus à l'extérieure"_ car elle permet de créer des structures _potentiellement_ infinies. Par contre il est possible que cette évaluation prenne plus de temps, par exemple dans le cas de l'évaluation de l'expression `square (3 + 4)`, une étape de plus est nécessaire plus que l'on duplique l'expression `3 + 4`. Heureusement Haskell utilise une astuce dans ce cas afin de faire pointer une expression dupliquée et ainsi n'avoir à évaluer l'expression qu'une fois.  
Haskell est dit _paresseux_ pour cette raison car une expression n'est réellement évaluée que si cela est nécessaire. Contrairement à d'autres langages dits _stricts_ (comme Java) où la création de structures potentiellement infinies est impossible.  
Essayez cela en Java par exemple :

{% highlight java %}
import java.util.ArrayList;
import java.util.List;

public class Infinity {

    public static void main(String[] args) {
        System.out.println(from(1).get(0));
    }

    /**
     * Append a value at the beginning of a given list.
     */
    private static <T> List<T> cons(T value, List<T> list) {
        final List<T> result = new ArrayList<>(list);
        result.add(0, value);

        return result;
    }

    /**
     * Return an infinite list of number starting from @from.
     */
    private static List<Integer> from(final int from) {
        return cons(from, from(from + 1));
    }
}
{% endhighlight %}

Heureusement depuis Java 8 il est possible de générer des flux infinis grâces aux streams :
{% highlight java %}
IntStream.iterate(1, i -> i + 1).limit(1).forEach(i -> System.out.println(i));
{% endhighlight %}

La deuxième vidéo reprenait l'utilisation des listes _potentiellement_ infinies pour les mettre en application afin de calculer la liste des nombres premiers en se basant sur le [crible d'Ératosthène], qui s'exprime très élégamment en Haskell. Cet algorithme indique que pour avoir la liste des entiers premiers, il suffit d'écrire la liste des entiers supérieur ou égal à 2, puis de supprimer les multiples du premier nombre, c'est-à-dire 2. Puis on recommence en supprimant tous les multiples du plus petit entier suivant restant (ici 3) et ainsi de suite (5, 7, 11, 13, ...).

{% highlight haskell %}
sieve :: [Int] -> [Int]
sieve (p : xs) = p : sieve [x | x <- xs, x `mod` p /= 0]

primes :: [Int]
primes = sieve [2..]
{% endhighlight %}


Pour conclure sur cette partie une série d'exercices étaient proposées. Si les premiers n'étaient pas réellement intéressants (identification d'expressions réductibles, comment sera évalué une expression) la seconde partie portant sur les listes infinies était elle très intéressante en permettant de voir en pratique leur utilisation.

## Raisonnement sur les programmes

La seconde partie demandait simplement de lire le chapitre correspondant du livre [Programming in Haskell] ! Heureusement ce chapitre avait été [mis en ligne gratuitement].  
Ce chapitre explique comment prouver un programme, en résolvant un cas de base (en général pour un nombre égal à 0 ou une liste vide) et en prouvant par [induction] que la propriété était vraie pour toute valeur.  
Les exercices permettait de mettre cela en pratique.

## Conclusion

Une avant-dernière semaine intéressante, permettant principalement de découvrir l'existence et l'intérêt des listes infinies (ou plus justement _potentiellement_ infinies).  La seconde partie sur le raisonnement était également intéressante, malheureusement l'équipe éducative s'est contentée de renvoyer au livre, ce qui ne me semble pas très motivant.  
La semaine prochaine je reviendrai sur le cours dans son ensemble afin de vous en livrer une évaluation (forcément subjective !)

[cours d'introduction à la programmation fonctionnelle]: https://www.edx.org/course/delftx/delftx-fp101x-introduction-functional-2126
[streams]: https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#package.description
[crible d'Ératosthène]: http://www.wikiwand.com/fr/Crible_d%27%C3%89ratosth%C3%A8ne
[Programming in Haskell]: http://www.cs.nott.ac.uk/~gmh/book.html
[mis en ligne gratuitement]: http://www.cs.nott.ac.uk/~gmh/chapter13.pdf
[induction]: http://www.wikiwand.com/fr/Raisonnement_par_r%C3%A9currence