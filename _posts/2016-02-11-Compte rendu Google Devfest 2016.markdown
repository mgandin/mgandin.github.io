---
layout: post
title:  "Compte rendu Google Devfest 2016"
date:   2016-02-11 15:00:00
categories: code
image: /assets/article_images/2016-02-11-Compte rendu Google Devfest 2016/img.jpg
---
Vendredi 5 février, j'ai eu l'occasion de participer au [Google DevFest 2016](http://devfest.gdgparis.com/) organisé à Paris, et voici mon petit compte-rendu.

Le temps de prendre un premier café, je me dirige vers la keynote d'ouverture présentée par Ludovic Cinquin, dont le sujet, les Géant du Web, ne m'est pas inconnu, puisque j'ai participé à l'écriture [de l'ouvrage](http://www.octo.com/fr/publications/11-les-geants-du-web) il y a quatre ans. Bonne présentation, même si on n'échappe pas au côté « Obvious » des messages qui sont là pour nous vendre un peu du rêve. Et je n'aurais pas été contre une petite mise à jour des pratiques, car il a dû s'en passer des choses depuis 2012.

J'enchaine avec le Hands-on sur Kubernetes avec Google Compute Engine et on est pas loin de l'échec avec cet atelier. Nos comptes Google Compute Engine n'était pas prêts et la séance s'est transformé en un gigantesque live-coding nous montrant l'exécution de commande Kubernetes, qui semble être un joli concurrent de [Docker Swarm](https://docs.docker.com/swarm/overview/). Quand je peux enfin me connecter au machine virtuel de Google – via mon téléphone, autant dire que je galère un peu – je ne réussi qu'à faire une portion des manipulations. Je récupère l'énoncé des exercices, j'essaierai de les refaire sur une machine avec Docker.

Pause le midi et je reprends avec une courte présentation de composants Polymer de reconnaissance d'écriture par Pierre-Alban Dewitte. La démo est impressionnante, le code m'a l'air très bon et les usages qui pourraient en émerger sont passionnants. Le code est [ici](https://github.com/myscript/myscript-math-web) et [là](https://github.com/myscript/myscript-text-web) et c'est plutôt pas mal.

Puis Julien Landuré revient sur ce qui va changer avec l'arrivée de HTTP2. Plutôt que de tout vous énumérer, j'ai envie de vous renvoyer vers la [spécification](https://httpwg.github.io/specs/rfc7540.html). A noter que Nginx est compatible HTTP2 depuis la version [1.9.5](https://www.nginx.com/blog/nginx-1-9-5/).

Je reprends du travail en cours pour finalement reprendre les conférence à 15h30. Je vais voir le live-coding de Thierry Chatel sur Angular 2. La présentation met en avant les bases du framework et déjà je sens déjà une fatigue m'envahir à l'utilisation de tout ces {, (, [ qui peuvent aussi se combiner façon ([]) ou {()}. Le code est [ici](https://github.com/tchatel/angular2-travels-devfest-paris-2016), et j'ai de ce pas essayer de le refaire chez moi. Sinon zéro plantage pendant le live-coding et c'est suffisamment rare pour être souligner.

Je termine avec le debuggage moderne de code JS dans le Chrome Dev Tools. Pas mal de choses intéressantes que je ne connaissais pas mais qui m'aurait été bien utiles, comme le support des Promises Javascript, ou encore l'intégration avec NodeJS. Le code est [là](https://github.com/jollivetc/debugJS), et cela fait aussi aussi partie des exercices à faire que je garde du Google Devfest.

Bilan : matinée très mitigée mais après-midi très bien. Google Devfest s'annonce déjà comme une belle conférence dont il faudra surveiller de près le programme des prochaines éditions.
