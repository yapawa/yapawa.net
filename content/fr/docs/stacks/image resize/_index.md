---
title: "Redimensionnement d'images"
linkTitle: "Redimensionnement d'images"
weight: 2
date: 2020-04-22
description: >
  Crée les vignettes à partir des arguments dans le chemin de la requête et les stocke pour les requêtes futures.
---
## Aperçu de l'infrastructure
{{< imgproc imageResize Resize "400x" "Aperçu de l'infrastructure" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>

## Pile

* Construit à l'aide de [serverless.com](https://serverless.com/)
* [S3](https://aws.amazon.com/s3/) pour le cache des vignettes
* [Cloudfront](https://aws.amazon.com/cloudfront/) pour la distribution
* [NodeJS](https://nodejs.org) [Lambda](https://aws.amazon.com/lambda/) avec [Sharp](https://github.com/lovell/sharp) pour la transformation des images

## Concept
L'idée est de servir les vignettes à un coût minimal.

Ce qui veut dire, sans l'utilisation de [Lambda@Edge](https://aws.amazon.com/lambda/edge/). Et sans devoir se connecter à une base de donnée.

## Variables d'environnement
* srcBucket: Le bucket gardant les images originales
* srcPrefix: Le prefix dans *srcBucket* où les images sont stockées
* cacheBucket: Le bucket stockant les images cachées
* cacheDomain: le domaine domain associé à la distribution Cloudfront

## Paramètres
Tous les paramètres sont passés par le chemin de la requête. Ceci simplifie les règles de caching sur le réseau de distribution.

`https://{domain}/{albumId}/{photoId}/{filename}/{version}/{transformations}/{name}.{format}`

* domain: Domaine Cloudfront
* albumId: L'identifiant de l'album de la photo (AlbumId)
* photoId: L'identifiant de la photo
* filename: Le nom de fichier de la photo
* version: Utilisé pour regénérer le cache
* transformations: (Voir le [Lisez moi du service](https://github.com/yapawa/imageResize/blob/master/README.md) pour plus de détails)
* name: Nom nettoyé de la Photo
* format: format de la photo (jpg, jpeg, png or webp)

En utilisant *srcBucket*, *srcPrefix*, *albumId*, *photoId* et *filename*, le service est capable de localiser l'image originale.

Pour éviter l'utilisateur de Lambda@Edge, le format doit être passé au service. Cela veut dire, que le client doit savoir ce qu'il supporte.
Autant [albumsManager](https://github.com/yapawa/albumsManager) que [publicsite-grid](https://github.com/yapawa/publicsite-grid) ont cette aptitude.

## Vignettes cachées

Les vignettes cachées sont stockées au même chemin que la requête. Ceci permet des facilement les servir depuis S3, sans le besoin d'une requête spécifique, si elle a déjà été générée.

## Déroulement

1. Le navigateur fait une requête pour une image
2. L'image est cachée dans Cloudfront: L'image est retournée. Le déroulement s'arrête.
3. L'image n'est pas cachée dans Cloudfront: Elle est récupérée depuis S3.
4. L'image existe dans S3, elle est retournée au client et cachée dans Cloudfront pour les requêtes futures. Le déroulement s'arrête.
5. L'image n'existe pas dans S3: S3 retourne au navigateur un statut "307 redirect" avec l'url de l'API HTTP.
6. Le navigateur fait une requête vers l'API HTTP.
7. La fonction Lambda récupère l'original, applique les transformations et sauve le résultat dans S3.
8. La fonction Lambda retourne au navigateur un statut "302 redirect" vers la requête Cloudfront d'origine.
9. Le navigateur recommence au point 1.
