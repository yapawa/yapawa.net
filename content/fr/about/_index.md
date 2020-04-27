---
title: A propos de Yapawa
linkTitle: A propos
menu:
  main:
    weight: 10
---
{{% blocks/cover title="A propos de Yapawa" height="auto" %}}
Yapawa est une collection de produits "serverless" sur AWS, permettant de gérer, héberger et distribuer vos galleries photo.

Continuez de lire pour en savoir plus, ou visitez notre [documentation](/docs/) pour démarrer!
{{% /blocks/cover %}}

{{% blocks/section color="200" type="section" %}}
## albumsManager

*  **Nécessaire**

Créez et géréez vore hiérarchie d'albums. Chargez vos images dans les albums. Assignez les images de couverture à vos albums.

* [AWS Amplify](https://aws-amplify.github.io/): Gestion de la pile
  * [Cognito](https://aws.amazon.com/cognito/): Identification d'utilisateurs
  * [Appsync](https://aws.amazon.com/appsync/): Point d'accès GraphQL
  * [DynamoDB](https://aws.amazon.com/dynamodb/): Stockage des données
  * [S3](https://aws.amazon.com/s3/): Hébergement du CMS et des images
  * [Cloudfront](https://aws.amazon.com/cloudfront/): Réseau de diffusion de contenu
  * [Lambda](https://aws.amazon.com/lambda/): Gestion dans le cloud de documents et des images
  * [Event Bridge](https://aws.amazon.com/eventbridge/): Gère le déploiement
* [Quasar](https.//quasar.dev/): VueJS Frontend Framework
* [ACM](https://aws.amazon.com/acm/): Certificats SSL
* [Route53](https://aws.amazon.com/route53/): DNS, validation des certificats
<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/albums manager/_index.md">}}">
  Plus <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/albumsManager">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section color="400" type="section" %}}
## imageResize

*  **Nécessaire**

Redimensionne, recadre et stocke les variations d'images.

* [Serverless.com](https://serverless.com/): Gestion de la pile
  * [API Gateway](https://aws.amazon.com/api-gateway/): Point d'entrée HTTPS
  * [S3](https://aws.amazon.com/s3/): Stockage et mise en cache des images redimensionnées
  * [Cloudfront](https://aws.amazon.com/cloudfront/): Réseau de diffusion de contenu
  * [Lambda](https://aws.amazon.com/cloudfront/): Redimensionne les images en utilisant [Sharp](https://sharp.pixelplumbing.com/)
  * [ACM](https://aws.amazon.com/acm/): Certificats SSL
  * [Route53](https://aws.amazon.com/route53/): DNS, validation des certificats

<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/image resize/_index.md">}}">
  Plus <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section color="300" type="section" %}}
## publicsite-grid

*  **Optionnel**

Héberge et livre la version publique.Host and serves a public version. Il fournit le contenu et la mise en page du site public.

* [Hugo](https://gohugo.io/): Générateur de site statique
* [S3](https://aws.amazon.com/s3/): Hébergement du site
* [Cloudfront](https://aws.amazon.com/cloudfront/): Réseau de diffusion de contenu
* [CodeBuild](https://aws.amazon.com/codebuild/): Compilation et deployment du site
* [ACM](https://aws.amazon.com/acm/): Certificats SSL
* [Cloudformation](https://aws.amazon.com/cloudformation/): Gère S3 et Cloudfront
* [Route53](https://aws.amazon.com/route53/): DNS, validation des certificats

<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/public site/_index.md">}}">
  Plus <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section type="section" color="white" %}}
## Pourquoi une autre galerie web ?

Il y a plusieurs services de galleries par le monde. Que ce soit des services de marques comme [GooglePhotos](https://www.google.com/photos/about/), [flickr](https://www.flickr.com/) ou [imgur](https://imgur.com/) pour en nommer que certains. Il y a aussi l'infini nombre de services à héberger soi-même.

Tous les services à héberger soi-même ont un point en commun, ils nécessitent des serveurs. Et de ce fait, ils ont besoin d'être maintenus et sécurisés.

Les services de marques manquent généralement de fonctionnalités, comme par exemple des albums hiérarchiques. Sans compte le fait, que les images ne sont plus les votres une fois chargés sur ces platformes.
Vous êtes également à la merci des stratégies de ventes, ces services peuvent disparaître aussi rapidement qu'ils sont arrivés. Souvenez vous de [Picasa](https://picasa.google.com/) ou [Panoramio](https://www.panoramio.com/).

Yapawa est né en prenant en considération les limites ci-dessus.

## Yapawa est "serverless"

Yapawa utilise les services "serverless" de [AWS](https://aws.amazon.com/), qui viennent avec les avantages suivants:

* Vous payez ce que vous utilisez. Vous de payez pas quand le service n'est pas utilisé.
* Augmentation automatique des capacités en cas d'augmentation des requêtes.
* Redondance de l'infrastructure et du réseau.

Pour être plus précis, vous ne payez pas 24/7/365 pour un CMS utilisé quelques heures par semaine ou un site publique recevant que quelques requêtes par heure. Mais si vous avez la chance d'être chanceux d'être cités quelque part et commencez à recevoir des centaines de requêtes à la seconde, le service va ajouter la capacité pour palier à la croissance.

## Site publique statique

Il classé comme optionnel, comme le CMS a une API publique, il peut facilement être remplacé par une SPA, autre modèle statique ou même un site en PHP (mais pourquoi s'infliger ceci?).

*publicsite-grid* est un site statique. Il est re-généré par le CMS après un changement. La capacité de calcul n'est utilisée qu'une fois, lorsqu'on crée les pages. Pour les servir, le HTML résultant est déjà crée et peux facilement être servi par un CDN.

{{% /blocks/section %}}
