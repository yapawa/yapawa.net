---
title: "Image Resize Setup"
linkTitle: "Image Resize"
weight: 3
date: 2020-04-20
description: >
  Install and configure *imageResize* to serve and cache thumbnails generated from the orginals images.
---
## Download Stack
```bash
git clone https://github.com/yapawa/imageResize.git
cd imageResize
nvm use
npm ci
cp -a stages/production.sample.yml stages/production.yml
```
## Configure Stack

Edit stages/production.yml:
```yml
profile: AWSProfileName
region: AWSRegion
suffix: "" # Or "-stage" for a multi stage deploy
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
* **cacheBucketName**: Bucket name of your choice
* **aws_user_files_s3_bucket**: Bucket created by [Albums Manager]({{< relref "albums-manager.md" >}})

## Deploy

```bash
npm run deploy
```

This will take some time, mainly due to the creation of the Cloudfront Distribution.

{{% alert title="S3 readiness" color="primary" %}}
If you deploy outside of *us-east-1*, your domain won't be accessible once the deploy is finished. You will need to wait up to one hour for the bucket redundancy to be ready. if you try to access you will be redirected to the Bucket webhosting endpoint. This is an [AWS issue](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), nothing that can be done.
{{% /alert %}}

{{% alert title="Multiple deployments" color="warning" %}}
If you want to deploy several stacks in a same AWS account, you need to create different stages files and deploy the stack for each of them.

```bash
npx sls -s stageName deploy
```
{{% /alert %}}

### What is created ?

* HTTP API
* S3 Bucket for cached images storage
* Lambda functions:
  * Image resizer using Sharp

## Next
* [Setup Public Site]({{< relref "public-site.md" >}})
