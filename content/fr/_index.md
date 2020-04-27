+++
title = "Yapawa"
linkTitle = "Yapawa"

+++

{{< blocks/cover title="Yet Another Photo Album Web App" image_anchor="top" height="full" color="orange" >}}
<div class="mx-auto">
  <a class="btn btn-lg btn-primary mr-3 mb-4" href="{{< relref "/about" >}}">
    En savoir plus <i class="fas fa-arrow-alt-circle-right ml-2"></i>
  </a>
  <a class="btn btn-lg btn-secondary mr-3 mb-4" href="https://github.com/yapawa">
    Github <i class="fab fa-github ml-2 "></i>
  </a>
  <p class="lead mt-5">Hébergement, Management, Distribution sans serveurs de vos images sur votre infrastructure</p>
  {{< blocks/link-down color="info" >}}
</div>
{{< /blocks/cover >}}


{{% blocks/lead color="primary" %}}
Yapawa est un système de gestion "serverless" d'images, un système d’hébergement et un système de distribution basé sur 3 structures indépendantes mais reliées.
{{% /blocks/lead %}}

{{< blocks/section color="dark" type="features">}}
{{% blocks/feature icon="fa-pencil-ruler" title="Gerstionnaire d'albums" url="/docs/stacks/albums-manager/" %}}
C'est le CMS. Vous l'utilisez pour créer et gérer la hiérarchie en arbre des albums. Vous charger des images dans les albums. Vous assignez des images de couverture aux albums.

Vous publiez les changements vers le site public en un seul click.
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-images" title="Redimensionnement d'images" url="/docs/stacks/image-resize/" %}}
Genère des vignettes des images originales basés sur les arguments dans l'URL. Les vignettes sont stockées pour de futures requêtes.
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-cloud" title="Site publique" url="/docs/stacks/public-site/" %}}
C'est la version que le monde voit. Vous pouvez utiliser le modèle fourni, ou créer le votre, du moment que vous arrivez à lire le contenu fourni par l'API.
{{% /blocks/feature %}}


{{< /blocks/section >}}
