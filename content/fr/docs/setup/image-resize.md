---
title: "Initialiser le redimensionnement d'images"
linkTitle: "Redimensionnement d'images"
weight: 3
date: 2020-04-20
description: >
  Installation et configuration du *redimensionnent d'images* pour servir et cacher les vignettes crées depuis les images originales.
---
## Télécharger la pile
```bash
git clone https://github.com/yapawa/imageResize.git
cd imageResize
nvm use
npm ci
cp -a stages/production.sample.yml stages/production.yml
```
## Configurer la pile

Éditer `stages/production.yml`:
```yml
profile: AWSProfileName
region: AWSRegion
suffix: "" # Ou "-stage" pour un déploiement de plusieurs phases
cacheBucket: cacheBucketName
srcBucket: <aws_user_files_s3_bucket>
srcPrefix: public # Important
domainName: img.gallery.example.com
hostedZoneId: hostedZoneId
certificateArn: arn:aws:acm:us-east-1:yyyyyyyy:certificate/zzzzzzz
lambda:
  memorySize: 1024
  timeout: 30
  logRetention: "30"
```
* **cacheBucketName**: nom du bucket de votre choix
* **aws_user_files_s3_bucket**: Bucket crée par [Gestionnaire d'albums]({{< relref "albums-manager.md" >}})

## Déploiement

```bash
npm run deploy
```

Ceci va prendre un peu de temps, principalement dû à la création de la distribution Cloudfront.

{{% alert title="Préparation de S3" color="primary" %}}
Si vous utilisez un autre région que *us-east-1*, votre domaine ne sera pas accessible de suite. Vous devrez attendre jusqu'à une heure pour que la redondance de S3 soit prête. Si vous essayzer d'accéder le domaine, vous serez redirigé vers le domaine du bucket S3. Ceci est une [limitation connue d'AWS](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), rien ne peut être fait.
{{% /alert %}}

{{% alert title="Plusieurs déploiements" color="warning" %}}
Si vous voulez déployer plusieurs piles dans le même compte AWS, vous devez créer plusieurs fichiers de phase et déployer pour chacune d'elle.

```bash
npx sls -s stageName deploy
```
{{% /alert %}}

### Qu'est-ce qui est crée ?

* HTTP API
* S3 Bucket pour le stockage des images cachées
* Fonction Lambda:
  * Redimensionnement d'image utilisant Sharp

## Suivant
* [Installation du site publique]({{< relref "public-site.md" >}})
