---
title: "Gestionnaire d'albums"
linkTitle: "Gestionnaire d'albums"
weight: 1
date: 2020-04-22
description: >
  C'est le CMS. Vous l'utilisez pour créer et gérer vos albums et la hiérarchie imbriquée. Charger vos images et les assigner aux albums. Assigner les couvertures d'albums.
  Vous déployer le site publique d'un seul click.
---
## Aperçu de l'infrastructure
{{< imgproc albumsManager Resize "400x" "Aperçu de l'infrastructure" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/albumsManager">
  Github <i class="fab fa-github ml-2 "></i>
</a>

## Frontend

* SPA écrit avec [VueJS](https://vuejs.org/) utilisant [Quasar](https://quasar.dev/) comme platforme
* [Amplify-Vue](https://docs.amplify.aws/ui-legacy/q/framework/vue) est utilisé pour améliorer les parties spécifiques à Amplify de l’interface utilisateur
* hébergé sur [S3](https://aws.amazon.com/s3/) et servi avec [Cloudfront](https://aws.amazon.com/cloudfront/)

### Nom de domaine
le nom de domaine est défini dans `amplify/team-provider-info.json`

### Images originales
Elles sont stockées dans un bucket dédié. L'acceleration de transfert est activée sur le bucket.

Les images sont chargées directement depuis le navigateur vers S3. Vous pouvez décider ou non d'utiliser l'acceleration de transfert dans `.env`.

### Internationalisation et localisation
3 langues sont fournies par défaut:
* Anglais
* Français
* Allemand

Si vous voulez supporter plus de langues, créez le fichier `src/i18n/<lang-iso>/index.js`. Ajoutez les traductions. Chargez les en ajoutant la référence à `src/i18n/index.js`.

Vous devez aussi fournir l'option dans `src/pages/Auth.vue` et `src/pages/Profile.vue`.

Si vous ajoutez une langue, un pull request est appréciée.

## Backend

Le backend est défini et provisionné en utilisant [AWS Amplify](https://aws.amazon.com/amplify/) et plus précisément [Amplify-CLI](https://docs.amplify.aws/cli).

### Authentification

Les informations d'identification des utilisateurs sont gérées par [Cognito](https://aws.amazon.com/cognito/).

En tirant parti de [Cognito's User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-cognito-user-pools.html)
vous pouvez rapidement prendre avantage d'un système d'identification d'utilisateur à part entière. Vous pouvez utiliser des SMS pour 2FA, récupérer le mot de passe, sauver des information complémentaires pour chaque utilisateur.

En ajoutant [Cognito's Identity Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html),
vous autorisez l'accès à chaque utilisateur à votre infrastructure AWS, les autorisant à interagir avec les services directement. Comme, par exemple, charger des fichiers sur S3 ou accéder à une API privée.

#### Informations des utilisateurs
* Nom d'utilisateur (champ par défaut)
* Mot de passe (champ par défaut)
* 2FA (optionnel)
* Langue de l'utilisateur (champ personnalisé)

### Stockage des informations complémentaires
Les information complémentaires des albums et photos sont stockées dans [DynamoDB](https://aws.amazon.com/dynamodb/).

L'accès à DynamoDB se fait par [AppSync](https://aws.amazon.com/appsync/) en utilisant GraphQL.

### Stockage des images
Les images sont stockées dans un bucket dédié et privé et ne sont pas accessible directement sans authentification.

#### EXIF
L'EXIF des images est extrait en utilisant une fonction [Lambda](https://aws.amazon.com/lambda/) et stockées dans un ficher JSON parallel dans S3. Cette fonction est déclenchée à chaque fois qu'une nouvelle image est sauvée dans S3.

#### Rotation des images
Les images peuvent être tournées (90° à droite ou 90° à gauche). Une fonction Lambda invoquée par AppSync va gérer la rotation de l'image de manière asynchrone. Le navigateur n'a pas besoin de charger l'image.

Une fois la rotation réussie, le navigateur va mettre à jour les données complémentaires en conséquence.

#### Vignettes

Elles sont crées par le service [Redimensionnement d'images]({{< relref "../image resize/_index.md" >}}).

Si vous voulez utiliser au autre service, vous devez adapter `src/mixins/thumbnails.js`.

## API publique

Après un changement sur un album ou une image, le navigateur va contacter AppSync pour initialiser la création d'un JSON statique à l'aide d'une fonction Lambda.

Les points d'entrée de l'API publique peuvent être utilisés par le site publique afin d'obtenir les informations à mettre en page.

### Points d'entrée

Les points d'entrée sont disponible seulement en GET et retournent un fichier de type *application/json*.

L'URL de base est identique pour tous les points d'entrée: *https://{Domaine du gestionnaire}/api/albums*

#### Liste

__/list__

Retourne une liste ordonnée des albums avec les informations détaillées pour chacun d'eux.

#### Arbre

__/tree__

Retourne une liste hiérarchique des albums avec les informations détaillées pour chacun d'eux.

#### Images

__/{albumId}/content__

Retourne une list ordonnée des images avec des informations détaillées pour chaque image.
