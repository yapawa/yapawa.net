+++
title = "Yapawa"
linkTitle = "Yapawa"

+++

{{< blocks/cover title="Yet Another Photo Album Web App" image_anchor="top" height="full" color="orange" >}}
<div class="mx-auto">
  <a class="btn btn-lg btn-primary mr-3 mb-4" href="{{< relref "/about" >}}">
    Learn More <i class="fas fa-arrow-alt-circle-right ml-2"></i>
  </a>
  <a class="btn btn-lg btn-secondary mr-3 mb-4" href="https://github.com/yapawa">
    Github <i class="fab fa-github ml-2 "></i>
  </a>
  <p class="lead mt-5">Serverless Hosting, Managing and Serving of your Images on your Infrastructure</p>
  {{< blocks/link-down color="info" >}}
</div>
{{< /blocks/cover >}}


{{% blocks/lead color="primary" %}}
Yapawa is a Serverless image management, hosting and distribution stack based on 3 independent but interlinked services.
{{% /blocks/lead %}}

{{< blocks/section color="dark" type="features">}}
{{% blocks/feature icon="fa-pencil-ruler" title="Albums Manager" url="/docs/stacks/albums-manager/" %}}
It's the CMS. You use it to create and manage a nested albums hierarchy. Upload and assign images to the albums. Assign cover images to the albums.

You deploy to your public site in a single click.
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-images" title="Image Resizer" url="/docs/stacks/image-resize/" %}}
Generates thumbnails from arguments in the request path and stores them to a cache for future requests.
{{% /blocks/feature %}}


{{% blocks/feature icon="fa-cloud" title="Public Site" url="/docs/stacks/public-site/" %}}
It is the version the world sees. You can use the provided static generated site, or replace it with your own. As long as you are able to read content from an API.
{{% /blocks/feature %}}


{{< /blocks/section >}}
