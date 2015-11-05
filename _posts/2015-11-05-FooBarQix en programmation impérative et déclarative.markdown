---
layout: post
title:  "FooBarQix en programmation impérative et déclarative"
date:   2015-11-05 10:00:00
categories: code
---

Cela fait déjà un an et demi que je suis (enfin) passé à Java 8 et, ma foi, je ne regrette pas le voyage. Java a (enfin) une librairie de Collection digne de ce nom (La librairie de Collection était déjà bien faite, mais il lui manquait des features qui existent déjà depuis de nombreuses années chez de nombreux langages). 

Mais surtout ça permet de ressortir ce bon vieux débat entre la programmation déclarative et impérative, notamment grace à l'apport des Streams et des lambdas au langage Java.

Je ne vous réexpliquerai pas en détail ce qu'est un stream ou un lambda, [d'autres l'ont déjà fait mieux que moi](https://www.parleys.com/tutorial/tout-ce-que-vous-avez-toujours-voulu-savoir-sur-les-lambdas-et-plus-encore-part-1), mais j'ai plutôt envie de prendre un petit exemple avec l'exercice du FooBarQix pour illustrer ça. 

### Le FooBarQix
Alors d'abord, c'est quoi l'exercice de FooBarQix ? Pour ceux qui l'ignore, il s'agit d'un bout de code un peu crétin qui consiste à afficher 100 nombres, mais avec quelques exceptions :

 * Quand on tombe sur un nombre qui est un multiple de 3 ou qui contient 3, on affiche FOO
 * Quand on tombe sur un nombre qui est un multiple de 5 ou qui contient 5, on affiche BAR
 * Quand on tombe sur un nombre qui est un multiple de 7 ou qui contient 7, on affiche QIX
 * Et c'est cumulatif (par exemple pour 35 on affiche BARQIXFOOBAR)

Il y a quelques années, cet exercice était utilisé par Facebook pour faire un premier tri dans les candidatures pour un poste de développeur. Depuis, on retrouve surtout cet exercice sur des posts de blogs pour illustrer quelques techniques de programmation, même si, FooBarQix n'apporte pas grand chose en terme de valeur.

### Commençons ce problème par les multiples de 3, 5 et 7 qui doivent afficher respectivement FOO, BAR et QIX
Essayons d'implémenter ça dans une approche pré-Java 8, cela donne ça :
{% highlight java %}
public String imperative(int toFooBar) {
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

On retrouve bien tous les signes de la programmation impérative, à savoir que ce code décrit toutes les opérations en séquences d'instructions exécutées pour modifier l'état du programme ([cf Wikipedia](https://fr.wikipedia.org/wiki/Programmation_imp%C3%A9rative)). C'est le mode de programmation le plus répandu aujourd'hui, et il suffit de lire le code pour comprendre comment il fonctionne.
Voyons maintenant ce que ça donne avec des streams et des lambdas :
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
Passons sur l'initialisation de la Map (pourra-t-on un jour faire Map foobar = (3 -> "FOO", 5 -> "BAR") ?) et regardons le reste. On est plutôt dans une forme de programmation déclarative où l'on se concentre sur le quoi (je filtre, je transforme et je collecte dans une chaine de caractère) et non sur le comment (avec des if/then/else). Ma foi c'est pas trop mal et ça se rapproche doucement de la programmation fonctionnelle.

Et bien sûr, ça se teste comme ça :
{% highlight java %}
@Test
public void should_make_the_foobar_test_with_contains() {
    FooBarModulo fooBarModulo = new FooBarModulo();
    for (int i = 1; i < 101; i++) {
        Assertions.assertThat(fooBarModulo.func(i)).isEqualTo(fooBarModulo.imperative(i));
    }
}
{% endhighlight %}

### Et maintenant occupons-nous des nombres qui contiennent 3, 5, 7 et qui doivent afficher FOO, BAR ou QIX
Dans une approche impérative voilà ce que ça donne :
{% highlight java %}
public String imperative(int toFooBar) {
    String result = "";
    String integer = String.valueOf(toFooBar);
    for (int j = 0; j < integer.length(); j++) {
        char element = integer.charAt(j);
        if(element == '3')
            result += "FOO";
        if(element == '5')
            result += "BAR";
        if(element == '7')
            result += "QIX";
    }
    return result.isEmpty() ? integer : result;
}
{% endhighlight %}

Et dans une approche plus déclarative :
{% highlight java %}
public String func(int numberToFooBar) {
    String toFooBar = String.valueOf(numberToFooBar);

    Map<Integer, String> foobar = new HashMap<>();
    foobar.put(3, "FOO");
    foobar.put(5, "BAR");
    foobar.put(7, "QIX");

    String result = toFooBar.chars()
        .mapToObj(integerAsChar -> foobar.getOrDefault(getNumericValue(integerAsChar), ""))
        .collect(joining());

    return result.isEmpty() ? toFooBar : result;
}
{% endhighlight %}

Et ça se teste comme ça :
{% highlight java %}
@Test
public void should_make_the_foobar_test_with_contains() {
    FooBarContains fooBarContains = new FooBarContains();
    for (int i = 1; i < 101; i++) {
        Assertions.assertThat(fooBarContains.func(i)).isEqualTo(fooBarContains.imperative(i));
    }
}
{% endhighlight %}

### Et pour finir
Rappelez vous de l'exercice du FooBarQix, il faut combiner le code qui teste si un nombre est multiple et s'il contient un chiffre, et c'est là que ça devient intéressant.
En programmation impérative, pré-Java8, sans surprise, le code décrit pas à pas chacune des instructions. Il suffit de lire le code sans trop réfléchir pour comprendre ce qu'il fait. On obtient ça :
{% highlight java %}
public String imperative(int numberToFoobar) {
    String result = "";
    if(numberToFoobar % 3 == 0) {
        result += "FOO";
    }
    if(numberToFoobar % 5 == 0) {
        result += "BAR";
    }
    if(numberToFoobar % 7 == 0) {
        result += "QIX";
    }
    String toFooBar = String.valueOf(numberToFoobar);
    for (int j = 0; j < toFooBar.length(); j++) {
        char element = toFooBar.charAt(j);
        if(element == '3')
            result += "FOO";
        if(element == '5')
            result += "BAR";
        if(element == '7')
            result += "QIX";
    }
    return result.isEmpty() ? toFooBar : result;
}
{% endhighlight %}

En utilisant les Streams et le lambdas, et en partant vers une programmation plus déclarative, on se retrouve avec du code plus abstrait, il faut comprendre ce qu'il y a derrière les mots "filter", "map" et "collect" mais Heureusement, ces opérations sont assez claires, et rendent finalement le code plus simple. 
{% highlight java %}
public String func(int numberToFooBar) {
    Map<Integer, String> foobar = new HashMap<>();
    foobar.put(3, "FOO");
    foobar.put(5, "BAR");
    foobar.put(7, "QIX");

    String result = foobar.keySet().stream()
            .filter(toReplace -> numberToFooBar % toReplace == 0)
            .map(foobar::get)
            .collect(joining());

    String toFooBar = String.valueOf(numberToFooBar);
    result += toFooBar.chars()
            .mapToObj(integerAsChar -> foobar.getOrDefault(getNumericValue(integerAsChar), ""))
            .collect(joining());

    return result.isEmpty() ? String.valueOf(toFooBar) : result;
}
{% endhighlight %}
Là où il y a encore à redire, c'est dans la connexion entre l'instruction qui vérifie le multiple et la suivante, Java ne propose malheureusement rien pour combiner ces deux bouts de code. On peut toujours les séparer dans plusieurs méthodes de classe :
{% highlight java %}
public String func(int numberToFooBar) {
    Map<Integer, String> foobar = fooBarQix();

    String result = contains(numberToFooBar, foobar);

    String toFooBar = String.valueOf(numberToFooBar);
    result += contains(foobar, toFooBar);
    return result.isEmpty() ? toFooBar : result;
}

private String contains(Map<Integer, String> foobar, String toFooBar) {
    return toFooBar.chars()
                .mapToObj(integerAsChar -> foobar.getOrDefault(getNumericValue(integerAsChar), ""))
                .collect(joining());
}

private String contains(int numberToFooBar, Map<Integer, String> foobar) {
    return foobar.keySet().stream()
                .filter(toReplace -> numberToFooBar % toReplace == 0)
                .map(foobar::get)
                .collect(joining());
}

private Map<Integer, String> fooBarQix() {
    Map<Integer, String> foobar = new HashMap<>();
    foobar.put(3, "FOO");
    foobar.put(5, "BAR");
    foobar.put(7, "QIX");
    return foobar;
}
{% endhighlight %}
Mais ça devient soudainement trop compliqué pour notre petit exercice de FooBarQix. Cependant, sur du code plus gros, c'est assurément une démarche à suivre. 

Mélanger programmation impérative, déclarative et orientée objet est finalement devenu le pain quotidien des développeurs qui utilisent Java 8 ... 

[Le code complet se trouve là](https://gist.github.com/mgandin/e56f811c613be121233c)








