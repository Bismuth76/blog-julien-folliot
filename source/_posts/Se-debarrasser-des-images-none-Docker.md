title: 'Se débarrasser des images <none> Docker'
date: 2016-04-02 12:35:11
categories:
- Serveur
- Docker
tags:
- Docker
- Reminder
---
{% asset_img docker-none-image.jpg Image Docker none %}

Il arrive parfois qu'une commande "`docker images`" vous affiche, dans la liste des images, une ou plusieurs images dont les champs `REPOSITORY` et `TAG` contiennent `<none>`.
<!-- more -->

Dans le cas où on passe le paramètre `-a` à cette commande, c'est normal, il s'agit d'images intermediaires générées par Docker et permettant un fonctionnement plus rapide.

En revanche, il arrive que certaines images ne soit pas correctement supprimées par Docker alors qu'elles n'ont plus d'utilité.

Dans ce cas, il suffit de simplement faire&nbsp;:

  ```javascript
  docker rmi $(docker images -f "dangling=true" -q)
  ```

Voilà, normalement, la commande `docker images` ne devrait plus laisser apparaître d'images "orphelines".

_Remarques&nbsp;: Si vous n'avez aucune image "polluante", Docker vous indiquera une erreur lorsque vous lancerez la commande magique (normal, dans ce cas vous ne lui indiquez aucune image à supprimer). Aucune inquiétude à avoir donc :)_

A bientôt&nbsp;!