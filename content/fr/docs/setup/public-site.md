---
title: "Installation du site publique"
linkTitle: "Site publique"
weight: 4
date: 2020-04-20
description: >
  Installation et configuration de *publicsite-grid*.
---
## Télécharger la pile

```bash
git clone https://github.com/yapawa/publicsite-grid
cd publicsite-grid
```

## Configurer la pile
Créez un fichier `.env`:
```bash
HUGO_TITLE="Titre du site"
HUGO_PARAMS_PAGETITLEPREFIX=""
HUGO_THEME="grid"
HUGO_LANGUAGECODE="fr"
HUGO_PARAMS_API="https://admin.gallery.example.com/api/" # Utilisez le domaine de votre gestionnaire d'albums
HUGO_PARAMS_CACHEDOMAIN="img.gallery.example.com" # Utilisez le domaine de votre redimensionneur d'images
HUGO_PARAMS_TAGLINE="Slogan su site"
HUGO_PARAMS_THEMECOLOR="white" # white ou grey
AWS_PROFILE=AWSProfile
AWS_REGION=AWSRegion
DOMAIN_NAME=gallery.example.com
CERTIFICATE_ARN=arn:aws:acm:us-east-1:XXXXXX:certificate/yyyyyyyy
HOSTED_ZONE_ID=HostedZoneId
HUGO_GOOGLEANALYTICS="UA-Tracking-Code"
WEBCLIENTID="webclientId" # aws_user_pools_web_client_id de albumsManager/src/aws-exports.js
```

* **WEBCLIENTID**: *aws_user_pools_web_client_id* du [Gestionnaire d'albums]({{< relref "albums-manager.md" >}}).

{{% alert title="Plusieurs déploiements" color="warning" %}}
Si vous voulez déployer plusieurs piles dans le même compte AWS, vous devez éditer `.env` avant de déployer chacun d'eux.
{{% /alert %}}

## Déployer la pile
```bash
bash setup.sh
```

Le projet CodeBuild est exécuté automatiquement une fois que la pile est provisionnée. Si vous ne le souhaitez pas, mettez la dernière ligne de *setup.sh* en commentaire.

{{% alert title="Préparation de S3" color="primary" %}}
Si vous utilisez un autre région que *us-east-1*, votre domaine ne sera pas accessible de suite. Vous devrez attendre jusqu'à une heure pour que la redondance de S3 soit prête. Si vous essayzer d'accéder le domaine, vous serez redirigé vers le domaine du bucket S3. Ceci est une [limitation connue d'AWS](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), rien ne peut être fait.
{{% /alert %}}

### Qu'est-ce qui est crée ?
* S3 Bucket
* Distribution Cloudfront
* Projet CodeBuild
  * Déclencheur depuis EventBridge
