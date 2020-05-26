---
title: "Albums Manager Setup"
linkTitle: "Albums Manager"
weight: 2
date: 2020-04-20
description: >
  Install and configure *albumsManager* CMS
---
## Download stack
```bash
git clone https://github.com/yapawa/albumsManager.git
cd albumsManager
nvm use
npm ci
```

## Project Name

The *projectName* is only used to name the Cloudformation stack.

The default project name *YapawaManager* can be changed by replacing *projectName* in `amplify/.config/project-config.json`.

## Initialize Amplify

Reply to the questions.

When asked about configuring a Cognito Lambda Trigger: **reply _no_**.

Else, use the default value when one is proposed.

```bash
amplify init
```

{{% alert title="Multiple deployments" color="warning" %}}
If you want to deploy several stacks in a same AWS account, you can use multiple environments.
{{% /alert %}}

## Add missing hosting configuration
We will be using S3 and Cloudfront to host our CMS and not [Amplify Console](https://aws.amazon.com/amplify/console/). S3 hosting doesn't allow to configure CNAME and certificates out of the box.
We need to add it manually to our configuration.

Edit `amplify/team-provider-info.json`. Under `prod.categories` add and entry for _hosting_, and replacing the values with the correct ones:
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

Create a `.env` file and define the domain that will be used by imageResize (*cacheDomain* variable). Decide also if you want to use [S3 Transfer Acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html) when uploading images:

```bash
cacheDomain=img.gallery.example.com
transferAcceleration=true
```

{{% alert title="Multiple deployments" color="warning" %}}
If you are using several stacks in the same AWS account, you need to edit `.env` before deploying each of them.
{{% /alert %}}

## Creating the stack and deploying the code

Tell amplify to publish your site. Amplify will automatically provision the stack.

```bash
amplify publish -c --yes
```

* `-c` tells Cloudfront to invalidate existing cache. But you still need to invalidate the cache in your Browser manually.
* `--yes` tells Amplify to answer automatically _yes_ to any question.

Be patient, this will take some time, mainly because of the Cloudfront distribution.

{{% alert title="S3 readiness" color="primary" %}}
If you deploy outside of *us-east-1*, your domain won't be accessible once the deploy is finished. You will need to wait up to one hour for the bucket redundancy to be ready. if you try to access you will be redirected to the Bucket webhosting endpoint. This is an [AWS issue](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), nothing that can be done.
{{% /alert %}}

Open `src/aws-exports.js` and store somewhere the value for *aws_user_files_s3_bucket*, you will need it when configuring [Image Resize]({{< relref "image-resize.md" >}}).

Open `src/aws-exports.js` and store somewhere the value for *aws_user_pools_web_client_id*, you will need it when configuring [Public Site]({{< relref "public-site.md" >}}).

### What is created ?

* Cognito User Pool
* Cognito Federated Identity
* Appsync Endpoint
* DynamoDB Table for Albums
* DynamoDB Table for Images
* Lambda functions:
  * Exif Reader
  * JSON Generator trigger
  * build JSON: album tree
  * build JSON: album details
  * Run Public Site build
  * Rotate Image
* S3 Bucket for image storage
* S3 Bucket for hosting
  * Triggers Lambda *Exif Reader* on PutObject
* Route53 CNAME
* Cloudfront Distribution for hosting
* Event Bridge: to dispatch build events

## Create a user
Open your browser and go to the [Cognito Console](https://eu-central-1.console.aws.amazon.com/cognito/users).

Open the created _User Pool_ and create a new user. This user will be the one used to administer the albums and images.

{{% alert title="Multiple users" color="primary" %}}
You can create more than one user. But each user will have full rights on all albums, images and deployments.
{{% /alert %}}

## Next
* [Setup Image Resize]({{< relref "image-resize.md" >}})
* [Setup Public Site]({{< relref "public-site.md" >}})
