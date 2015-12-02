---
layout: post
title:  "Land of Lisp (en Clojure (Chapitre 2))"
date:   2015-12-01 22:00:00
categories: code
---

Je ne sais pas pour vous, mais je n'ai pas gardé un souvenir folichon de mes cours sur la programmation fonctionnelle. Ajouté à cela, ça remonte à très (très) loin dans ma mémoire la programmation fonctionnelle et que le sujet commence à revenir à la mode, entre Scala qui n'en finit plus de prendre de l'essor et Java 8 qui, depuis l'arrivée de Streams, s'adapte tout doucement à ce paradigme, j'ai eu envie de replonger doucement dans le sujet.

Pour cela j'ai décidé de commencer [« Land Of Lisp »](http://landoflisp.com/), dont la lecture se révèle être, chose rare pour un livre d'informatique, assez fun. Afin de corser un peu le challenge, j'ai aussi décidé de reprendre les exercices du livre en Clojure, qui, vous allez vite le voir, diffère un peu du Lisp.

Après un premier chapitre introductif, [« Land Of Lisp »](http://landoflisp.com/) démarre sur quelques bases du langage, en proposant de développer un petit jeu où il faut faire deviner un nombre entre 1 et 100 par le programme tout en lui en indiquant si le nombre en question est plus grand ou plus petit que celui qui est proposé.

La solution en Lisp se trouve [par ici](http://landoflisp.com/guess.lisp) et voici comment je me suis débrouillé pour refaire ce petit jeu en Clojure. 

Pour commencer je définis deux variables qui stockent les nombres min et max du jeu :
{% highlight clojure %}
(def *small* 1)
(def *big* 100)
{% endhighlight %}

Ensuite j'écris une fonction qui devine un nombre (on ajoute *big*  et *small* que l'on divise par deux et on garde l'arrondi) :
{% highlight clojure %}
(defn guess [] 
  (Math/round (double(/ (+ *big* *small*) 2))))
{% endhighlight %}

Puis j'ajoute une fonction qui propose un nombre plus grand en changeant *small* par le résultat de l'appel de la fonction guess (et je rappelle alors guess pour proposer un nombre) :
{% highlight clojure %}
(defn bigger []
  (def *small*(guess))
  (guess))
{% endhighlight %}

Je fait ensuite à peu près la même chose pour un nombre plus petit, en changeant cette fois-ci *big* :
{% highlight clojure %}
(defn smaller []
  (def *big* (guess))
  (guess))
{% endhighlight %}

Ensuite je joue (mettons que le programme doit deviner 42) :

 * (guess) me retourne 51
 * J'appelle (smaller) qui me retourne 26
 * J'appelle (bigger) qui me retourne 39, puis une seconde fois pour me retourner 45
 * J'effectue un dernier appel sur (smaller) qui me retourne enfin 42

Ca fonctionne et voici le code complet :
{% highlight clojure %}
(def *small* 1)

(def *big* 100)

(defn guess [] 
  (Math/round (double(/ (+ *big* *small*) 2))))

(defn smaller []
  (def *big* (guess))
  (guess))

(defn bigger []
  (def *small*(guess))
  (guess))
{% endhighlight %}

[Le code complet se aussi trouve là](https://gist.github.com/mgandin/87441a88aa057c9ee6d9)








