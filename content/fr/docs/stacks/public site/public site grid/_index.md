---
title: "Site publique Grid"
linkTitle: "Site publique Grid"
weight: 3
date: 2020-04-22
description: >
  C'est le modèle par défaut. C'est très simpliste et minimalist. Le but est qu'il soit léger et rapide à afficher.
---
## Aperçu de l'infrastructure

{{< imgproc publicSiteGrid Resize "400x" "Aperçu de l'infrastructure" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>

*publicsite-grid* est le modèle par défaut. Il permet d'afficher vos images de manière simpliste et minimalist.

## Pile

Le site utilise [Hugo](https://gohugo.io/), un générateur de site statique, pour créer à l'avance toutes les pages HTML nécessaires.

Les images sont servies en utilisant le [Redimensionnement d'images]({{< relref "../../image resize/_index.md">}}). Le modèle doit connaître la structure du chemin utilisée.

## Construction

La pile crée un projet [CodeBuild](https://aws.amazon.com/codebuild/), qui est lancé sur en évènement de [EventBridge](https://aws.amazon.com/eventbridge/) envoyé par l'utilisateur dans le [gestionnaire d'albums]({{< relref "../../albums manager/_index.md" >}}).

## Schéma de couleur

Le modèle est fournit avec 2 schéma de couleurs: fond blanc (par défaut) ou fond gris. la valeur (white / grey) est définie dans `config.toml`.

## Albums et images
Un script NodeJS ([makeContent.js](https://github.com/yapawa/publicsite-grid/blob/master/makeContent.js)) lit l'entrée /tree de l'API publique.

A l'aide de ces données, il va créer une *page* pour chaque album en respectant la hiérarchie et l'ordre.

Pour les collections, il crée un dossier. Pour les albums, il va lire le contenu de l'album de l'API publique et générer le code Markdown.

Hugo va créer le contenu HTML du site.

## Masonry Grid


Javascript n'est pas utilisé pour générer la grille, uniquement CSS, en utilisant [CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout).

## Responsive

Le modèle est totalement "responsive". Il utilise toute la place disponible, que ce soit sur un mobile ou un fixe.

[Webp](https://developers.google.com/speed/webp) est utilisé quand possible. la densité de l'écran et plusieurs règles de tailles permettent un affichage optimal des images.

## Lightbox

[PhotoSwipe](https://photoswipe.com/) est utilisé pour fournir un effet "lightbox" aux images, avec la capacités d'utiliser le gestes habituels.

## AMP

Le modèle produit 2 différentes versions: une classique et une en [AMP](https://amp.dev/). Les deux versions sont très similaires, vu que la version classique est déjà très minimaliste et optimisée pour un affichage mobile.

## SEO

LD+JSON et plusieurs données OpenGraph sont fournies pour améliorer le SEO.

## Ajouter d'autres thèmes Hugo

*Grid* est un thème pour Hugo. N'importe quel autre thème pourrait y être ajouté. PR sont bienvenus.

Le thème à utiliser peut être défini dans `config.toml`.

{{% alert title="Nom du dépôt" color="secondary" %}}
Clairement le suffixe du dépôt "-grid" ne serait pas logique si plus de thèmes sont mis à disposition.
{{% /alert %}}
