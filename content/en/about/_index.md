---
title: About Yapawa
linkTitle: About
menu:
  main:
    weight: 10
---
{{% blocks/cover title="About Yapawa" height="auto" %}}
Yapawa is a collection of serverless stacks on AWS, allowing you to manage, host and serve your photo galleries.

Read on to find out more, or visit our [documentation](/docs/) to get started!
{{% /blocks/cover %}}

{{% blocks/section color="200" type="section" %}}
## albumsManager

*  **Needed**

Create and manage nested albums hierarchy. Upload images to the albums. Assign cover images to the albums.

* [AWS Amplify](https://aws-amplify.github.io/): Stack Orchestration
  * [Cognito](https://aws.amazon.com/cognito/): User Login
  * [Appsync](https://aws.amazon.com/appsync/): GraphQL Endpoint
  * [DynamoDB](https://aws.amazon.com/dynamodb/): Data storage
  * [S3](https://aws.amazon.com/s3/): CMS Hosting, Image storage
  * [Cloudfront](https://aws.amazon.com/cloudfront/): CDN delivery
  * [Lambda](https://aws.amazon.com/lambda/): Server side files and image manipulations
  * [Event Bridge](https://aws.amazon.com/eventbridge/): Handles deploy events
* [Quasar](https.//quasar.dev/): VueJS Frontend Framework
* [ACM](https://aws.amazon.com/acm/): SSL certificate
* [Route53](https://aws.amazon.com/route53/): DNS, certificate validation
<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/albums manager/_index.md">}}">
  More <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/albumsManager">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section color="400" type="section" %}}
## imageResize

*  **Needed**

Resizes, crops and stores image variations.

* [Serverless.com](https://serverless.com/): Stack Orchestration
  * [API Gateway](https://aws.amazon.com/api-gateway/): HTTPS Endpoints
  * [S3](https://aws.amazon.com/s3/): Storage and caching of resized images
  * [Cloudfront](https://aws.amazon.com/cloudfront/): CDN delivery
  * [Lambda](https://aws.amazon.com/cloudfront/): Resize Images using [Sharp](https://sharp.pixelplumbing.com/)
  * [ACM](https://aws.amazon.com/acm/): SSL certificate
  * [Route53](https://aws.amazon.com/route53/): DNS, certificate validation

<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/image resize/_index.md">}}">
  More <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section color="300" type="section" %}}
## publicsite-grid

*  **Optional**

Host and serves a public version. It provides the content and layout for the public site.

* [Hugo](https://gohugo.io/): Static site generator
* [S3](https://aws.amazon.com/s3/): Hosting of the site
* [Cloudfront](https://aws.amazon.com/cloudfront/): CDN delivery
* [CodeBuild](https://aws.amazon.com/codebuild/): Website building and deploy
* [ACM](https://aws.amazon.com/acm/): SSL certificate
* [Cloudformation](https://aws.amazon.com/cloudformation/): Manages S3 and Cloudfront
* [Route53](https://aws.amazon.com/route53/): DNS, certificate validation

<div>
<a class="btn btn-sm btn-primary mr-3 mb-4" href="{{< relref "../docs/stacks/public site/_index.md">}}">
  More <i class="fas fa-arrow-alt-circle-right ml-2 "></i>
</a>
<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>
</div>
{{% /blocks/section %}}

{{% blocks/section type="section" color="white" %}}
## Why Another Web Gallery ?

There a several web galleries services out in the world. Be it branded services like [GooglePhotos](https://www.google.com/photos/about/), [flickr](https://www.flickr.com/) or [imgur](https://imgur.com/) to name a few. Or the endless amount of self hosted services.

All the self hosted services, have a point in common, they need servers to run. And therefore they need servers to be maintained and secured.

The branded services lack functionalities, like hierarchical albums. Not including the fact, that your pictures are no more totally yours once uploaded to them. You are also at the mercy of their marketing strategies. The services can go away as fast as they arrived, just remember [Picasa](https://picasa.google.com/) or [Panoramio](https://www.panoramio.com/).

Yapawa was born with the above limitations in mind.

## Yapawa is serverless

It uses serverless services from [AWS](https://aws.amazon.com/) which comes with following benefits:

* Pay for what you use, don't pay for idle time
* Automatically scale for increased requests
* Hardware and network redundancy

More specifically, you don't pay for a 24/7/365 server for a CMS used a few hours a week or for a public site receiving only a few hits an hour. But in the lucky case, you are feature somewhere and start receiving hundreds of requests a second, the service will scale and cope with the increase.

## Static public site
It is classified as *optional*, since the CMS has a public API and therefore could be easily replaced by an SPA, and other static template or even a PHP site (but why would you do that to yourself).

*publicsite-grid* is a static site. It is re-generated using the CSM API after a change. CPU is only used once, when generating the pages. To serve them , the resulting HTML is already created and can easily leverage a CDN.

{{% /blocks/section %}}
