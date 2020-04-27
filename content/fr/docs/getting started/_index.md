---
title: "Guide de démarrage"
linkTitle: "Guide de démarrage"
weight: 1
date: 2020-04-20
description: >
  Cette page vous indiquera comment démarrer avec Yapawa. Ce qu'il faut préparer avant d'installer les services.
---
## Prerequisites

* Compte AWS
* Utilisateur IAM avec des droits *Power user*
    {{% alert title="Droits requis" color="warning" %}}
pas tous els services sont nécessaires. Mais il y a suffisamment de services pour lesquels un accès est nécessaire, pour que l'utilisation de *Power User* simplifie la configuration au détriment de la sécurité.
    {{% /alert %}}
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
* [Profiles AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
* [nvm](https://github.com/nvm-sh/nvm/blob/master/README.md)
* NodeJS: `nvm install node`

## Domaines
Les domaines doivent être hébergés dans [Route53](https://aws.amazon.com/route53/). Cloudformation va automatiquement crées les alias requis.

Vous devez définir 3 domaines. Un pour chaque service. Pour simplifier, il est recommendé d'utiliser des sous-domaines. Mais ce n'est pas nécessaire.

Par exemple:
* gestionnaire d'albums: admin.gallery.example.com
* Redimensionnement d'images: img.gallery.example.com
* Site publique: gallery.example.com

### ID de la zone hébergée
Vous devez récupérer la *HostedZoneId* pour chaque domaine. Vous allez devoir la copier dans la configuration de chaque service.

```bash
HOSTEDZONEID=$(aws --profile myprofile route53 list-hosted-zones --query 'HostedZones[?Name==`example.com.`].Id' --output text | cut -d'/' -f3)
```
{{% alert title="Console AWS" color="primary" %}}
Vous pouvez aussi récupérer la hostedZoneId depuis la [console AWS](https://console.aws.amazon.com/route53/home?#hosted-zones:)
{{% /alert %}}
{{% alert title="Plusieurs domaines" color="warning" %}}
Si vous décider d'utiliser différent domaines de base pour vos services, vous devez récupérer la HostedZoneId de chaque domaine.
{{% /alert %}}

## Certificats ACM
Les [certificates ACM](https://aws.amazon.com/acm/) pour [Cloudfront](https://aws.amazon.com/cloudfront/) doivent être générés dans la région **us-east-1**.
Et comme [Cloudformation](https://aws.amazon.com/cloudformation/) est incapable de facilement définir des infrastructures dans plusieurs régions dans une même définition,
nous allons créer les certificats manuellement at copier leurs ARN comme référence dans la configuration des services.

[Ouvrez ACM](https://console.aws.amazon.com/acm/home?region=us-east-1#/) dans **us-east-1**, générez un certificat unique pour *gallery.example.com* comme domaine principal et *\*.gallery.example.com* comme domaine alternatif.
Utilisez la validation DNS, cliquez sur le bouton afin de créer des entrées dans Route53.

Mémoriser l'ARN (arn:aws:acm:us-east-1:*AccountId*:certificate/*certificateId*) pour son utilisation future.

Attendez que les certificats soient validés.
{{% alert title="Plusieurs domaines" color="warning" %}}
Si vous décidez d'utiliser plusieurs de base pour vos services, vous devez créer plusieurs certificats et mémoriser tous les ARN.
{{% /alert %}}

## Amplify
Si vous n'avez pas encore installé [Amplify](https://aws-amplify.github.io/):
```bash
npm install -g @aws-amplify/cli
amplify configure
```

## Region AWS
Les services ne doivent pas nécessairement être dans la même région. Mais c'est fortement recommandé, spécialement pour le *Redimensionnement d'images* et le *site publique*.

## Suivant
* [Installation du gestionnaire d'albums]({{< relref "albums-manager.md" >}})
* [Installation du redimensionnement d'images]({{< relref "image-resize.md" >}})
* [Installation du site publique]({{< relref "public-site.md" >}})
