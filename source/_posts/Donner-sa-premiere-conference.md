title: Donner sa première conférence
date: 2015-07-07 20:52:11
tags: Blabla
categories:
- Blabla
---

J'ai eu la chance de donner une petite conférence, ma première, dernièrement (le 2 juillet 2015...) dans le cadre du MeetUp organisé par SQLI Nantes. Suite à ça, je me suis dit que ça pouvait être intéressant de vous faire partager quelques uns des points à prendre en compte si vous devez présenter quelque chose.
<!-- more -->

## Quel support ?

Dans un premier temps, il est important de savoir sur quel support on veut présenter ses slides.

Pour ma part, j'ai été un poil trop ambitieux, j'ai présenté mes slides dans une application Polymer (mon sujet c'était les webcomponents et j'affichais des composants dans mes slides : une map Google, des composants customs, etc) et ça s'est avéré extrêmement lourd à mettre en place (j'ai finalement passé plus de temps à développer le moteur de slides que le contenu de la présentation... J'éxagère mais on n'est pas loin de ça :D).

Ce que je veux dire c'est que si vous n'avez pas de besoins particuliers, préférez les solutions du marché type **PowerPoint** ou **Impress**. Si vous voulez quelque chose de plus "sympa", **RevealJS** est vraiment pas mal.

## Quelques points techniques

D'autre part, il y a quelques points techniques auquels il vaut mieux faire attention :

- Résolution du projecteur
- Ratio du projecteur (4/3 ou 16/9 ?)
- Entrée vidéo du projecteur si vous amenez votre ordinateur (VGA, DVI, HDMI, etc)
- Si PowerPoint, compatibilité de version
- Si RevealJS, attention au poste sur lequel vous allez présenter (si ce n'est pas le vôtre).

De manière générale, histoire d'assurer le coup, n'hésitez pas à préparer plusieurs formats de support (mieux vaut trop que pas assez), comme par exemple, faire un export pdf de la prés' en 4/3 :)

## Le contenu

De manière générale, épurez au maximum votre support de présentation. Plus l'information affichée est verbeuse, moins le publique est attentif à ce que vous dites.

>Un dessin vaut mieux qu'un long discours

Soyez **synthétique**, **clair** et **précis** (facile à dire évidemment...).

Dans l'idéal, soyez drôle. Une petite blague par-ci, par-là, c'est toujours sympa (dans vos slides mais aussi dans votre discours). En revanche, adaptez vos blagues à votre publique, sans pour autant vous interdire une ou deux private jokes (par private joke, j'entends blague à destination d'une partie du publique, pas une blague entre votre copin(e) et vous-même, cela va de soi).

Ah, une petite note, plus à moi-même qu'autre chose. Il est important d'assumer ses blagues. Lors de ma présentation, je finissais chacune de mes blagues en baissant la tête :/

Et un dernier petit point, j'adore les animations mais au final, mieux vaut vous cantonner à un passage de slide classique et sans fioriture plutôt que d'imaginer quarante-douze animations qui ne feront qu'embrouiller votre auditoire qui découvre votre support pour la première fois.

## Le contenu complexe

Parfois, il est nécessaire d'afficher un tableau, un bon gros bout de code, un graphique à 37 dimensions, etc. Dans ces cas là, n'hésitez pas à faire découvrir l'information au fur et à mesure.

Par exemple, un bout de code complexe pourrait être découpé en plusieurs slides en partant d'un grand conteneur vide qu'on remplit au fur et à mesure :

**Slide 1**

  ```javascript
  function toto() {




  }
  ```

**Slide 2**

  ```javascript
  function toto() {
    var titi = 0;



  }
  ```

**Slide 3**

  ```javascript
  function toto() {
    var titi = 0;
    for(var i=0; i<43; i++) {

    }
  }
  ```

**Slide 4**

  ```javascript
  function toto() {
    var titi = 0;
    for(var i=0; i<43; i++) {
      titi += 3*L_AGE_DU_CAPITAINE;
    }
  }
  ```

De cette façon, le publique a peu d'informations à analyser et reste plus facilement focalisé sur ce que vous dites (qui pour rappel est **synthétique**, **clair** et **précis**).

## Pour résumer

...voici les quelques points clefs que je retiendrai pour mes futures conférences (s'il y a) :

- Bien préparer ses slides
  - Préparer un ou deux formats de supports
  - Pas de contenu trop exhaustif (vous êtes l'animateur de la conférence, vos slides ne sont qu'une accroche visuelle)
  - Quelques blagues (même pourries) à droite, à gauche
  - Être **synthétique**, **clair** et **précis** :)
- Bien se préparer
  - Répéter tout seul
  - Répéter devant quelques personnes se rapprochant du publique visé (une fois au moins) et prenez note du temps que vous mettez à présenter
  - Identifiez les slides sur lesquels vous pouvez aller plus vite ou prendre plus de temps afin de vous donner de la marge (on n'a pas le même débit de parole quand on est en condition réelle et on n'est pas à l'abri d'un trou de mémoire)
  - Allez-y sereinement (avec vos supports toussatoussa) et dites-vous que les gens à qui vous allez vous adresser ont choisi d'être ici (et donc intéressés par votre sujet) et sont probablement des gens comme vous (j'entends par-là qu'ils vont aux toilettes, etc)

Je vous invite à jeter un oeil aux différentes conférences (dont la mienne) dans cette [vidéo YouTube](https://www.youtube.com/watch?v=OKg9gIs6W0Y). Chaque conférence dure un quart d'heure et traitent de :
- Application Kiosk sur Android
- Les Webcomponents
- Angular 2
- Les logs
- Go et Rust

Sur ce, à plus ;)
