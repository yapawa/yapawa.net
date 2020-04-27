---
title: "Migration depuis Koken"
linkTitle: "Migration depuis Koken"
weight: 1
date: 2020-04-22
description: >
  Migration depuis le CMS Koken vers Yapawa
---
[Koken](http://koken.me) est un CMS pour les photographes écrit en PHP.

Effectuer une migration d'un CMS vers un autre n'est jamais simple. C'est la raison pour laquelle cet outil existe.

## Koken vers Yapawa

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/koken-to-yapawa">
  Github <i class="fab fa-github ml-2 "></i>
</a>

Cet outil exporte tous les albums publiés en utilisant l'API de Koken. La hiérarchie et l'ordre sont respectés et les données respectives sont sauvés dans les tables de Yapawa.

Les images originales sont transférées dans le bucket S3.

Les images de couvertures sont assignées en conséquence.

## Serverless

L'ensemble du processus est "serverless". La pile est orchestrée par [serverless.com](https://serverless.com/) et utilise:
* [AWS Step Function](https://aws.amazon.com/step-functions/)
* [Lambda](https://aws.amazon.com/lambda/)

## Configuration et utilisation

Se référer au [Lisez moi](https://github.com/yapawa/koken-to-yapawa/blob/master/README.md) du projet.

## Avertissements
* Uniquement les albums et images publiques sont transférées.
* Les données EXIF sauvegardées en dehors des images ne sont pas copiées. Uniquement les données EXIF disponibles dans l'image originale seront utilisées.
* Le point de focale de l'image n'est pas copié.
* Le contenu text de Koken n'est pas copié.
