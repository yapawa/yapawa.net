---
title: "Installation du gestionnaire d'albums"
linkTitle: "Gestionnaire d'albums"
weight: 2
date: 2020-04-20
description: >
  Installation et configuration du CMS *Gestion des albums*
---
## Télécharger la pile
```bash
git clone https://github.com/yapawa/albumsManager.git
cd albumsManager
nvm use
npm ci
```

## Nom du project

Le nom du projet (*projectName*) est seulement utilisé pour nommer le projet Cloudformation.

Le nom par défaut *YapawaManager* peut être changé en remplaçant *projectName* dans `amplify/.config/project-config.json`.

## Initialiser Amplify

Répondez aux questions.

Quand vous êtes demandés à propos de Cognito Lambda Trigger: **répondez _non_**.

Sinon, utiliser les valeurs par défaut proposées.

```bash
amplify init
```

{{% alert title="Déploiements multiples" color="warning" %}}
Si vous voulez déploier plusieurs piles dans le même compte AWS, vous pouvez utiliser plusieurs environnements.
{{% /alert %}}

## Ajouter la configuration d’hébergement manquante
On va utiliser S3 et Cloudfront pour héberger notre CMS et pas la [Console Amplify](https://aws.amazon.com/amplify/console/). L'hébergement S3 ne permet de configurer un CNAME et des certificats lors de la configuration par Amplify.
On doit l'ajouter manuellement à notre configuration.

Éditez `amplify/team-provider-info.json`. Sous `prod.categories` ajoutez une entrée pour *hosting*, et remplacez les valeurs ci-dessous avec les correctes:
```json
{
  "prod": {
    "awscloudformation": {
      ...
    },
    "categories": {
      ...,
      "hosting": {
        "S3AndCloudFront": {
          "domainName": "admin.gallery.example.com",
          "hostedZoneId": "XXXXXXXXXX",
          "certificateArn": "arn:aws:acm:us-east-1:abcdef:certificate/yyyyyyy"
        }
      }
    }
  }
}
```

Créez un fichier `.env` et défionissez le domaine qui sera utilisé pour le redimensionnement des images (la variable *cacheDomain*). Décidez aussi si vous voulez utiliser [S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html) quand vous chargez vos images:

```bash
cacheDomain=img.gallery.example.com
transferAcceleration=true
```

{{% alert title="Plusieurs déploiements" color="warning" %}}
Si vous utiliser plusieurs piles dans le meme compte AWS, vous devez éditer `.env` avant de
If you are using several stacks in the same AWS account, you need to edit `.env` avant de déployer chacun d'eux.
{{% /alert %}}

## Création de la pile et déployer le code

Dites à Amplify de publier votre site. Amplify va automatiquement provisionner l'infrastructure nécessaire.

```bash
amplify publish --yes
```

Soyez patient, cela va prendre un peu de temps. Spécialement en raison de la création de la distribution Cloudfront.

{{% alert title="Préparation de S3" color="primary" %}}
Si vous utilisez un autre région que *us-east-1*, votre domaine ne sera pas accessible de suite. Vous devrez attendre jusqu'à une heure pour que la redondance de S3 soit prête. Si vous essayzer d'accéder le domaine, vous serez redirigé vers le domaine du bucket S3. Ceci est une [limitation connue d'AWS](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), rien ne peut être fait.
{{% /alert %}}

Ouvrez `src/aws-exports.js` et sauvegardez quelque part la valeur de *aws_user_files_s3_bucket*, vous en aurez besoin quand vous installez le [redimensionnement d'images]({{< relref "image-resize.md" >}}).

Ouvrez `src/aws-exports.js` et sauvegardez quelque part la valeur de *aws_user_pools_web_client_id*, vous en aurez besoin quand vous installez le [site publique]({{< relref "public-site.md" >}}).

### qu'est-ce qui est crée ?

* Cognito User Pool
* Cognito Federated Identity
* Appsync Endpoint
* Table DynamoDB pour les albums
* Table DynamoDB pour les images
* Fonctions Lambda:
  * Lecteur Exif
  * Déclencheur du générateur JSON
  * Construire JSON: arbre des albums
  * Construire JSON: détails des albums
  * Lancer la génération du site publique
  * Rotation des images
* S3 Bucket pour le stockage des images
* S3 Bucket pour l'hébergement
  * Déclenche Lambda *Exif Reader* sur un PutObject
* Route53 CNAME
* Distribution Cloudfront pour l'hébergement
* Event Bridge: pour déclencher la création du sute publique

## Créer un utilisateur
Ouvrez votre navigateur et allez sur la [console Cognito](https://eu-central-1.console.aws.amazon.com/cognito/users).

Ouvrez le _User Pool_ crée et ajoutez un nouvel utilisateur. Cet utilisateur sera le gérant des albums et des images.

{{% alert title="Plusieurs utilisateurs" color="primary" %}}
Vous pouvez créer plusieurs utilisateurs. mais chacun d'eaux aura un accès complet à tous les ablums, images et au déploiement vers le site publique.
{{% /alert %}}

## Suivant
* [Installation du redimensionnement d'images]({{< relref "image-resize.md" >}})
* [Installation du site publique]({{< relref "public-site.md" >}})
