title: Tilde indexOf()
date: 2015-07-29 20:48:00
tags:
categories:
- Javascript
---
Je vous présente ici un petit tuyau qui vous permettra de briller en société et accessoirement de rendre votre code un tantinet illisible pour le développeur qui n'est pas du matin.

Allez hop ! Je vous mets le petit bout de code qui illustre mes dires :

Au lieu d'écrire :

  ```javascript
  if('toto'.indexOf('t') > -1) {
    console.log('Voilà !');
  }
  ```
Ou encore :

  ```javascript
  if('toto'.indexOf('t') >= 0) {
    console.log('Voilà !');
  }
  ```

Vous pouvez écrire :

  ```javascript
  if(~'toto'.indexOf('t')) {
    console.log('Voilà !');
  }
  ```

**Voilà !**

C'est plus concis, moins habituel et c'est pas jojo... Mais c'est stylé :D

<!-- more -->

Pour la petite explication, le `~` (tilde) est l'opérateur bitwise qui permet de faire les transformations suivantes :

**~(-2)** devient **1**
**~(-1)** devient **0**
**~ 0**   devient **-1**
**~ 1**   devient **-2**
**~47**   devient **-48**

Ainsi, notre indexOf renvoyant `-1` s'il ne trouve rien, en passant par ce mécanisme, on récupère `0`. Là où je trouve ça limite, c'est que quand il trouve quelque chose, on se retrouve avec des valeurs négatives...

Pour plus d'information sur le pourquoi du comment, vous pouvez jeter un oeil sur [le complément à deux](https://fr.wikipedia.org/wiki/Compl%C3%A9ment_%C3%A0_deux).

A plus ;)
