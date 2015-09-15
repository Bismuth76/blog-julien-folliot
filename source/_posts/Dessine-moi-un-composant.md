title: Dessine-moi un composant
date: 2015-08-27 22:52:09
tags:
- Tuto
- Webcomponents
- Polymer
- Javascript
- HTML
- CSS
categories:
- Polymer
---

{% asset_img logo.svg Logo webcomponents.org %}

On entend de plus en plus parler des Webcomponents sans vraiment savoir de quoi il s'agit (plus creux comme phrase d'intro, tu meurs !).

Cet article est le premier d'une série de tutoriels sur l'utilisation de Polymer dans lequel je vais tâcher de vous présenter les tenants et les aboutissants de cette nouvelle notion qui risque de devenir le standard de demain sur le web.
<!-- more -->

>S'il vous plaît... dessine-moi un composant !

...dit le Petit Prince.

Le Petit Prince c'est moi, c'est vous, c'est quiconque ayant déjà eu affaire à un design demandant de modifier le rendu ou même le comportement d'une `checkbox` ou d'un `select`...

Si, comme le Petit Prince, tout ce qu'offre le HTML ne vous convient pas ou ne couvre pas votre besoin, les Webcomponents vont tenter d'y répondre.

>Voilà une boite ! Ton composant est dedans...

..répondit-on au Petit Prince.

C'est effectivement ce qu'on nous propose avec les Webcomponents. Plutôt que de jongler avec l'existant, on va pouvoir écrire des choses de ce genre :

```html
<mon-select values="['toto', 'titi']"></mon-select>
```

>Mouais... Si c'est juste pour pouvoir faire des balises customs, une directive Angular fait très bien l'affaire...

Certes ! Mais les Webcomponents ne se limitent pas à la création de balises personnalisées.

## C'est quoi un webcomponent ?

Si je devais résumer les webcomponents en quelques mots, je choisirais les suivants :
- Isolation
- Réutilisabilité
- Sémantique

**Isolation ?** Effectivement, un composant va pouvoir être isolé et ne pas être impacté par les autres éléments de la page qu'il s'agisse de script, de style ou de DOM.

**Réutilisabilité ?** Parce-qu'un des buts des Webcomponents, ça va être le partage. On va pouvoir utiliser des composants clefs-en-main sans avoir à se soucier de ce qu'il y a dedans. L'exemple le plus intéressant est celui de la GoogleMap. Habituellement, il faut charger une librairie, implémenter une balise donnée et faire un peu de JS pour avoir une map qui fonctionne. Avec les Webcomponents, je vais pouvoir écrire :

```html
<google-map latitude="37.77493" longitude="-122.41942"></google-map>
```

Et voilà, j'ai une map qui s'affiche comme il faut ! C'est tellement simple que c'en est déconcertant...

**Sémantique ?** Oui, je parle de sémantique parce que, finalement, voir `<google-map>` dans un fichier html, c'est plus lisible et compréhensible que `<div id="ma-gmap">` (sans compter qu'il y'a un bout de script dans un coin pour l'initialisation dans le dernier cas...).

Et au-delà de la lecture du fichier par un développeur, il ne faut pas oublier que la sémantique est très importante au niveau du référencement auprès des moteurs de recherche.

## Comment ça marche ?

Pour que tout ça fonctionne, il a fallu mettre en place un certain nombre de mécanismes. 4 pour être précis :
- Template
- Custom Elements
- Shadow DOM
- HTML Imports

Ces mécanisme font l'objet de spécifications W3C (pour les plus curieux : [specs de Template](https://html.spec.whatwg.org/multipage/scripting.html#the-template-element), [des Custom Elements](http://w3c.github.io/webcomponents/spec/custom/), [du Shadow DOM](http://w3c.github.io/webcomponents/spec/shadow/) et [des HTML Imports](http://w3c.github.io/webcomponents/spec/imports/)). Ces spécifications sont bien entendu à l'état de draft au moment où j'écris ces lignes...

## Qu'est-ce donc qu'un template ?

La notion de template, dans les grandes lignes, est identique à toutes celles qu'on peu rencontrer habituellement. Le but du jeu c'est d'avoir un morceau de HTML qu'on peut cloner à souhait et réutiliser partout dans notre code. On dira de ce morceau de code qu'il est inerte, on alimentera ensuite son contenu après l'avoir cloné.

L'exemple fourni par les specs est parlant, je vais donc vous le copier ici et le **détailler plus bas** (en plus, ici c'est mieux que dans les specs, y'a de la couleur :P). Attention, on ne parle pas encore de template à la sauce Polymer, ici on est sur du "standard" :

```html
<!DOCTYPE html>
<title>Cat data</title>
<script>
 // Data is hard-coded here, but could come from the server
 var data = [
   { name: 'Pillar', color: 'Ticked Tabby', sex: 'Female (neutered)', legs: 3 },
   { name: 'Hedral', color: 'Tuxedo', sex: 'Male (neutered)', legs: 4 },
 ];
</script>
<table>
 <thead>
  <tr>
   <th>Name <th>Colour <th>Sex <th>Legs
 <tbody>
  <template id="row">
   <tr><td><td><td><td>
  </template>
</table>
<script>
 var template = document.querySelector('#row');
 for (var i = 0; i < data.length; i += 1) {
   var cat = data[i];
   var clone = template.content.cloneNode(true);
   var cells = clone.querySelectorAll('td');
   cells[0].textContent = cat.name;
   cells[1].textContent = cat.color;
   cells[2].textContent = cat.sex;
   cells[3].textContent = cat.legs;
   template.parentNode.appendChild(clone);
 }
</script>
```


Prenons le temps d'analyser étape par étape.


### La base de la base

```html
<!DOCTYPE html>
<title>Cat data</title>
```
Eh non, le DOCTYPE n'a pas bougé. Pour ceux qui débutent vraiment, ce DOCTYPE indique "juste" au navigateur d'interpréter la page comme étant du HTML5. Nul besoin de spécifier quoi que ce soit en rapport avec les webcomponents.

Ensuite, on a le titre qui nous permet de savoir qu'on va parler de chats.

### Quelques données

```html
<script>
 // Data is hard-coded here, but could come from the server
 var data = [
   { name: 'Pillar', color: 'Ticked Tabby', sex: 'Female (neutered)', legs: 3 },
   { name: 'Hedral', color: 'Tuxedo', sex: 'Male (neutered)', legs: 4 },
 ];
</script>
```

On initialise ici un jeu de données juste pour tester. Comme le dit si bien le commentaire, ces données peuvent venir d'ailleurs (d'un serveur par exemple).

On notera que la pauvre Pillar n'a que 3 pattes :(

### Ça y est, on parle de template !

```html
<table>
 <thead>
  <tr>
   <th>Name <th>Colour <th>Sex <th>Legs
 <tbody>
  <template id="row">
   <tr><td><td><td><td>
  </template>
</table>
```

Dans un premier temps, on affiche un tableau (`<table>`) avec un en-tête (`<thead><tr><th>Name <th>Colour <th>Sex <th>Legs`) puis un `<tbody>` contenant... Notre `<template>` !

Penchons nous un peu dessus. On voit que ce template ne contient que des balises vides (`<tr><td><td><td><td>`). Comme je vous disais plus haut, un template est un morceau de HTML inerte qui ne contient donc aucune donnée (en dehors des données statiques si on en avait).

Si on lançait notre page avec seulement les trois blocs que je vous ai présenté jusqu'à maintenant, on verrai que notre `<tbody>` est vide. Le dernier morceau de code que je vais vous présenter va permettre de changer ça.

### Sortons notre template de l'inertie

```html
<script>
 var template = document.querySelector('#row');
 for (var i = 0; i < data.length; i += 1) {
   var cat = data[i];
   var clone = template.content.cloneNode(true);
   var cells = clone.querySelectorAll('td');
   cells[0].textContent = cat.name;
   cells[1].textContent = cat.color;
   cells[2].textContent = cat.sex;
   cells[3].textContent = cat.legs;
   template.parentNode.appendChild(clone);
 }
</script>
```

Voyons voir un peu ce qu'il se passe... La ligne 2 nous permet de récupérer notre template en utilisant simplement le `querySelector` standard (ce n'est pas une nouveauté qui vient avec les webcomponents, c'est déjà très bien implémenté et cela nous permet de manipuler tout élément du DOM).


Nous allons ensuite boucler sur notre tableau de chats (personnellement, j'aurais fait un `data.forEach` mais bon...). A chaque tour on va :
- ligne 4 : stocker le chat sur lequel on boucle
- ligne 5 : récupérer un clone du contenu de notre template
- ligne 6 : récupérer la liste des éléments `<td>` dans notre clone
- lignes 7 à 10 : alimenter chaque `<td>` avec les données de notre chat
- ligne 11 : l'étape la plus importante, on va ajouter notre clone dans le parent du template, à savoir le `<tbody>`. Il est important de noter qu'ils ont simplifié la démarche pour l'exemple, on aurait très bien pu mettre notre clone dans un autre élément de la page en le sélectionnant via un querySelector.

**Voilà ! ** Vous savez maintenant ce qu'est un template ! L'exemple présenté est très simple et l'intérêt des templates ne saute pas forcément aux yeux mais je vous garantis que c'est cool et nous verrons plus tard que c'est vraiment fabuleux :)

Je tiens à vous rassurer, c'était le chapitre le plus verbeux, la suite devrait être plus courte. Allez, parlons un peu de...


## Custom Elements

La seconde spec dont je vous ai parlé plus haut c'est Custom Elements.

Custom Elements va définir deux choses majeures :

- La possibilité de créer une balise avec le nom qu'on veut (ou presque)
- Le cycle de vie d'un webcomponent

*Je dis 2 majeures parce qu'il y a quelques éléments supplémentaires comme l'enregistrement d'un nouvel élément ou encore l'héritage, je vous laisse jeter un oeil aux specs pour plus d'infos.*

### Créer une balise `<jacquie-et-michel>`

Eh oui, tout est possible. Enfin tout ou presque... Pour être valide, votre balise doit :

- Contenir au moins un trait d'union (ou tiret haut si vous préférez)
- Ne pas porter un de ces noms (parce qu'ils sont déjà utilisés) :
    - annotation-xml
    - color-profile
    - font-face
    - font-face-src
    - font-face-uri
    - font-face-format
    - font-face-name
    - missing-glyph

Ah et enfin, même si ce n'est pas interdit, éviter d'appeler une balise par le même nom qu'une autre (pour l'instant, il n'y a pas grand monde mais à l'avenir, les risques de conflits de noms risque d'augmenter...).

Dans l'idéal, préfixez vos webcomponents d'un identifiant qui vous est propre. A titre d'exemple, les éléments liés à firebase sont tous préfixés par firebase (vous pouvez vérifier [ici](https://elements.polymer-project.org/elements/firebase-element?active=firebase-collection) ).

### Le cycle de vie

Comme je l'ai dit un peu plus haut, les spécifications des custom elements indiquent le cycle de vie d'un webcomponent. Sans rentrer dans les détails, voilà à peu près à quoi ça ressemble :

- Création de l'élément
- L'élément est attaché (entendez par là qu'il a été ajouté à un endroit dans la page)
- Un des attributs de l'élément a changé (initialisé, modifié ou supprimé)
- L'élément a été détaché

Vous voyez, je vous avais dit que cette partie serait plus courte :)

> Next !

## Shadow DOM

Bon par contre, là, je ne garantis pas que ça va être court...

Je vais commencer par un exemple concret. Vous connaissez sans doute la balise `<video>` apparue avec HTML5. Si on inspecte le DOM d'une page Youtube ([celle-ci par exemple](https://www.youtube.com/watch?v=DIp7Q6vAMqE)\) et qu'on se rend au niveau de la balise vidéo, voilà ce qu'on peut voir (pour ceux qui ne voit pas comment faire l'inspection, commencer par faire un `F12` pour afficher la console puis cliquez sur la petite loupe en haut à gauche de la console et enfin, cliquez sur l'encart de la vidéo) :

{% asset_img video-shadow-dom-invisible.png Visualisation de la balise vidéo dans le DOM %}

On voit notre balise vidéo. Eh bien sans le savoir, vous êtes devant un webcomponent ! Restons sur notre page et rendons-nous dans les paramètres de la console (la petite roue dentée en haut à droite). Vous allez cocher la case **Show user agent shadow DOM** :

{% asset_img parametres-console-chrome.png Paramètres de la console de Chrome %}

Puis refermez la console et observez à nouveau la balise `<video>`. Vous pouvez desormais l'ouvrir et voir ce qu'il s'y passe !

{% asset_img video-shadow-dom-visible.png Visualisation de la balise vidéo dans le DOM avec le Shadow DOM %}

Eh bien le shadow DOM c'est ça, c'est un document isolé du reste de la page. Le concept va beaucoup plus loin que ça (dans les specs, vous pourrez lire la façon dont sont gérés les arbres de documents, la transmission des événements, etc, etc) mais c'est en gros ce qui va permettre de cantonner nos informations à notre composant.

Pour information, par opposition au **Shadow DOM**, le DOM visible est appelé **Light DOM** (et Polymer a mis en place une espèce d'hybride des deux qui a été baptisé **Shady DOM**, mais nous verrons cela ensemble plus tard...).

Allez, plus qu'un petit effort et je vous laisse tranquille ! Passons au plus simple...

## HTML Imports

Cette spec est la plus courte et surtout celle que je peux vous résumer très rapidement :

> Les imports HTML permettent d'importer du HTML.

**Voilà ! **

On va donc pouvoir importer depuis une source, un bout de code HTML (au hasard, un webcomponent ^^).

Un des intérêts de ces imports c'est qu'ils vont nous permettre de gérer nos dépendances intelligemment. Si j'ai un `<composant-alpha>` et que deux de mes autres composants `<composant-beta>` et `<composant-gamma>` ont besoin de mon `<composant-alpha>` *(un petit air de ["laisser passer A38"](https://www.youtube.com/watch?v=DTqvL74dAEw) )*, alors ils vont tous les deux faire un import du `<composant-alpha>` mais ce dernier ne sera en réalité importé qu'une seule fois. Plutôt cool non ?

## Le mot de la fin

Ce qu'il faut retenir de tout ça c'est qu'on va bientôt pouvoir créer des composants réutilisables dans tout l'internet du monde ~~et de Navarre~~ et partager nos plus belles implémentations de Nyan Cat !

Pourquoi je dis bientôt ? Parce qu'aujourd'hui (en 2015 pour celui qui lira cette article en 2020 :P), on ne peut pour ainsi dire pas profiter pleinement de tout le potentiel des webcomponents étant donnée l'absence quasi totale d'implémentation des specs (qui pour rappel sont en draft) **en revanche**, on peut d'ores et déjà s'en rapprocher fortement grâce à **Polymer** et c'est ce que nous verrons dans les prochains articles !

Alors à très vite !

Bisous bisous ( *coeur avec les doigts* )