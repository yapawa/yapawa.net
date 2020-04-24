---
title: "Image Resize"
linkTitle: "Image Resize"
weight: 2
date: 2020-04-22
description: >
  Generates thumbnails from arguments in the request path and stores them to a cache for future requests.
---
## Infrastructure Overview
{{< imgproc imageResize Resize "400x" "Infrastructure Overview" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>

## Stack

* Build with [serverless.com](https://serverless.com/) framework
* [S3](https://aws.amazon.com/s3/) for thumbnails cache
* [Cloudfront](https://aws.amazon.com/cloudfront/) for delivery
* [NodeJS](https://nodejs.org) [Lambda](https://aws.amazon.com/lambda/) with [Sharp](https://github.com/lovell/sharp) for image manipulation

## Concept
The idea is to serve resized images at a minimum cost.

This means, without the need of a [Lambda@Edge](https://aws.amazon.com/lambda/edge/) function. Without the need to connect to a database.

## Environment variables
* srcBucket: The bucket holding the originals images
* srcPrefix: The prefix in the *srcBucket* where the images are stored
* cacheBucket: The bucket storing the cached thumbnails ()
* cacheDomain: The domain associated to the Cloudfront distribution

## Parameters
All parameters are passed within the path, this ensures the easiest CDN caching mechanism.

`https://{domain}/{albumId}/{photoId}/{filename}/{version}/{transformations}/{name}.{format}`

* domain: Cloudfront domain
* albumId: The AlbumId of the photo
* photoId: The Id ID of the photo
* filename: Photo source filename
* version: allows cache busting
* transformations: (See [service Readme](https://github.com/yapawa/imageResize/blob/master/README.md) for details)
* name: Photo slug
* format: output format (jpg, jpeg, png or webp)

By using *srcBucket*, *srcPrefix*, *albumId*, *photoId* and *filename*, the service is able to locate the original image.

To avoid using a Lambda@Edge, the output format need to passed to the service. This means that the client needs to know what he can support. Both [albumsManager](https://github.com/yapawa/albumsManager) and [publicsite-grid](https://github.com/yapawa/publicsite-grid) have this ability.

## Cached thumbnail

The cached thumbnail will be stored at the same path as the request path. This allows to easily serve it from the S3 bucket, without a specific lookup, if it was already generated.

## Flow

1. Browser requests for an image
2. The image is cached in Cloudfront: image is served. And flow stops.
3. The image is not cached is Cloudfront: It is fetched from S3.
4. The image exists in S3, it is returned to the client and cached in Cloudfront for future requests. The flow stops.
5. The Image doesn't exist in S3: S3 will return a 307 redirect with the HTTP API URL to the browser
6. The browser requests the image on HTTP API
7. The Lambda function fetches the original, applies the transformations and stores the result to S3.
8. The Lambda function returns a 302 redirect to the original Cloudfront request
9. The browser goes back to point 1
