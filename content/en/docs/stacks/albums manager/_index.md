---
title: "Albums Manager"
linkTitle: "Albums Manager"
weight: 1
date: 2020-04-22
description: >
  It’s the CMS. You use it to create and manage a nested albums hierarchy. Upload and assign images to the albums. Assign cover images to the albums.
  You deploy to your public site in a single click.
---
## Infrastructure Overview
{{< imgproc albumsManager Resize "400x" "Infrastructure Overview" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/albumsManager">
  Github <i class="fab fa-github ml-2 "></i>
</a>

## Frontend

* SPA written in [VueJS](https://vuejs.org/) using [Quasar](https://quasar.dev/) as framework
* [Amplify-Vue](https://docs.amplify.aws/ui-legacy/q/framework/vue) is used to enhance the Amplify specific parts of the UI (Mainly Authorization)
* Hosted on [S3](https://aws.amazon.com/s3/) and served with [Cloudfront](https://aws.amazon.com/cloudfront/)

### Domain Name
The domain name is set in `amplify/team-provider-info.json`

### Original Images
They are stored in a dedicated bucket. Transfer Acceleration is enabled on the bucket.

Images are uploaded directly from the browser to S3. You can choose to use or not Transfer Acceleration by editing the value in `.env`.

### Internationalization and localization
3 languages are provided by default:
* English
* French
* German

If you want to support more languages, create the file `src/i18n/<lang-iso>/index.js`. Add the translations. Load it, by adding it to `src/i18n/index.js`.

You will also need to provide the option in `src/pages/Auth.vue` and `src/pages/Profile.vue`

If you add a language, a pull request would be appreciated.

## Backend

The backend is defined and provisioned using [AWS Amplify](https://aws.amazon.com/amplify/) and more specifically [Amplify-CLI](https://docs.amplify.aws/cli).

### Authentication

User credentials are managed by [Cognito](https://aws.amazon.com/cognito/).

By leveraging [Cognito's User Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/getting-started-with-cognito-user-pools.html)
you quickly can make use of a fully fledged user credentials system. Allowing you to use SMS based OTP, reset passwords, store additional informations.

By adding [Cognito's Identity Pool](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html),
you can grant access to every single user to AWS infrastructure, allowing him to interact directly with them. Like uploading files to S3 or accessing a private API.

#### User Informations
* User Name (default field)
* User password (default field)
* User OTP (optional)
* User Language (custom field)

### Metadata Storage
Album's and Photo's metadata are stored in [DynamoDB](https://aws.amazon.com/dynamodb/).

Access to DynamoDB is done via [AppSync](https://aws.amazon.com/appsync/) using GraphQL.

### Images Storage
Images are stored in a dedicated private bucket and aren't accessible directly without authentication.

#### EXIF
Images EXIF data is extracted using a [Lambda](https://aws.amazon.com/lambda/) and stored as a sidecar JSON. This function is triggered each time a new file is stored in S3.

#### Image rotation
Images can be rotated (90° right or 90° left). A Lambda function invoked via an AppSync endpoint will carry out the rotation asynchronously. The browser doesn't need to transfer the image.

Once the rotation has succeeded, the browser will update the metadata accordingly.

#### Thumbnails

Theses are generated by the [Image Resize]({{< relref "../image resize/_index.md" >}}) service.

If you want to use another service, you need to adapt `src/mixins/thumbnails.js`

## Public API

After any change on an Album or an Image, the browser will call an AppSync Endpoint to initiate a static JSON generation with a Lambda function.

This public API endpoints can be used by the public site, to get informations about the content to display.

### Endpoints

The endpoints are available only with GET and return an *application/json* object

The basepath is common to all endpoints: *https://{Manager Domain}/api/albums*

#### List

__/list__

Returns an ordered list of albums with metadata for each album.

#### Tree

__/tree__

Returns a nested list of albums with metadata for each album.

#### Images

__/{albumId}/content__

Returns an ordered list of images with metadata for each image.