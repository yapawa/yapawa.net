---
title: "Bienvenue sur Yapawa"
linkTitle: "Documentation"
menu:
  main:
    weight: 20
---
Bienvenue sur le guide de Yapawa. Ce guide, va vous aider à configurer et publier les services centraux.

Il vous expliquera aussi comment gérer vos albums et images et les publier vers votre site publique.

Vous trouverez aussi de l'aide sur comment créer votre propre site publique.

## Qu'est-ce que Yapawa ?
Yapawa est un acronyme pour "Yet Another Photo Album Web App". Qui se traduit par "Encore une autre application Web d'album photo" (EUAAWAP ?). Comme le nom le suggère, ce n'est qu'une autre application pour publier ses images en tant que site web.

Caractéristiques spécifiques à Yapawa:
* Serverless: Utilisation de services "serverless" spécifique de [AWS](https://aws.amazon.com)
* Séparer les applications selon les activités qu'ils gèrent:
  * CMS
  * Redimensionnement d'images
  * Site public
* Niveaux imbriqués d'albums infinis
* Toutes les images peuvent être utilisées comme couvertures d'albums

## Est-que Yapawa est pour moi ?
Vous devez posséder des connaissances techniques basiques. L'installation de Yapawa n'est pas du type click-suivant.

Vous devez aussi posséder:

* A compte AWS avec un utilisateur ayant des droits administratifs.
* Connaissances basiques de AWS et de ses services.
* [AWS CLI](https://aws.amazon.com/cli/) configuré avec un [profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html).
* Connaissance basique de l'utilisation de commandes dans un terminal.
* Aptitude d'installer des paquets npm globalement sur votre machine de développement.

Yapawa vous permet uniquement de créer des albums et des images. Les vidéos ne sont pas supportées. Votre contenu ne peut être publié que pour le monde entier.

Si vous voulez partager des albums de manière privée, Yapawa n'est pas pour vous.

Toute personne avec un accès au CMS, aura accès aux albums et images. Si vous voulez que chaque administrateur gère ses propre images dans une installation commune, Yapawa n'est pas pour vous.

## Prêt à démarrer ?
Suivez le [guide de démarrage]({{< relref "getting started/_index.md" >}}) ou lisez les descriptions détaillées de chaque service.
