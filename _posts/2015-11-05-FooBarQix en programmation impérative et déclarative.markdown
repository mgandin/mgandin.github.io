---
layout: post
title:  "FooBarQix avec les streams et les lambdas"
date:   2015-08-28 14:00:00
categories: code
---


Cela fait déjà un an que je suis (enfin) passé à Java 8 et, ma foi, je ne regrette pas le voyage. Java a (enfin) une librairie de Collection digne de ce nom grace à l'apport des Streams et des lambdas (je trolle un peu, la librairie de Collection était déjà bien, mais il lui manquait des features qui existent déjà depuis de nombreuses années chez de nombreux langages).

Je ne vous réexpliquerai pas en détail ce qu'est un stream ou un lambda, [d'autres l'ont déjà fait mieux que moi](https://www.parleys.com/tutorial/tout-ce-que-vous-avez-toujours-voulu-savoir-sur-les-lambdas-et-plus-encore-part-1), mais j'ai plutôt envie de reprendre un petit exemple pour illustrer ça. 

Prenons l'exercice de FooBarQix. Alors d'abord, c'est quoi l'exercice de FooBarQix ? Pour ceux qui l'ignore, il s'agit d'un bout de code un peu crétin qui consiste à afficher 100 nombres, mais avec quelques exceptions :
* Quand on tombe sur un nombre qui multiple de 3 ou qui contient 3, on affiche FOO
* Quand on tombe sur un nombre qui multiple de 5 ou qui contient 5, on affiche BAR
* Quand on tombe sur un nombre qui multiple de 7 ou qui contient 7, on affiche QIX
* Et c'est cumulatif (par exemple pour pour 35 on affiche BARQIXFOOBAR)

Pour info, cet exercice était utilisé par Facebook pour faire un premier tri dans le nombre de candidatures pour un poste de développeur. Depuis, ils ont arrêté, mais l'exercice est utilisé depuis déjà un certain temps pour illustrer quelques techniques de programmation, même si, FooBarQix n'apporte pas grand chose en terme de valeur.

Alors essayons de faire FooBarQix en Java 8 avec des Streams et des Lambdas. 
### H3 Commençons ce problème par les multiples de 3, 5 et 7 qui doivent afficher respectivement FOO, BAR et QIX
Essayons d'implémenter ça dans une approche pré-Java 8, cela donne cette méthode :
{% highlight java %}
public String ugly(int toFooBar) {
    String result = "";
    if(toFooBar % 3 == 0) {
        result += "FOO";
    }
    if(toFooBar % 5 == 0) {
        result += "BAR";
    }
    if(toFooBar % 7 == 0) {
        result += "QIX";
    }
    return result.isEmpty() ? String.valueOf(toFooBar) : result;
}
{% endhighlight %}

On parle ici de programmation impérative, à savoir que ce code décrit toutes les opérations en séquences d'instructions exécutées pour modifier l'état du programme ([cf Wikipedia](https://fr.wikipedia.org/wiki/Programmation_imp%C3%A9rative)). C'est le mode de programmation le plus répandu aujourd'hui, voyons ce que ça donne avec des streams et des lambdas :
{% highlight java %}
public String func(int toFooBar) {
    Map<Integer, String> foobar = new HashMap<>();
    foobar.put(3, "FOO");
    foobar.put(5, "BAR");
    foobar.put(7, "QIX");

    String result = foobar.keySet().stream()
            .filter(toReplace -> toFooBar % toReplace == 0)
            .map(foobar::get)
            .collect(joining());

    return result.isEmpty() ? String.valueOf(toFooBar) : result;
}
{% endhighlight %}
Passons sur l'initialisation de la Map (pourra-t-on un jour faire Map foobar = (3 -> "FOO", 5 -> "BAR") ?) et regardons le reste, on est plutôt dans une forme de programmation déclarative où l'on se concentre sur le quoi (je filtre, je transforme et je collecte dans une chaine de caractère) et non sur le comment (avec des if/then/else). Ma foi c'est pas trop mal et ça se rapproche doucement de la programmation fonctionnelle.







