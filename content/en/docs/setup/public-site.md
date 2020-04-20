---
title: "Public Site Setup"
linkTitle: "Public Site"
weight: 4
date: 2020-04-20
description: >
  Install and configure *publicsite-grid*.
---
## Download Stack

```bash
git clone https://github.com/yapawa/publicsite-grid
cd publicsite-grid
```

## Configure Stack
Create a `.env` file:
```bash
HUGO_TITLE="Site Title"
HUGO_PARAMS_PAGETITLEPREFIX=""
HUGO_THEME="grid"
HUGO_LANGUAGECODE="en"
HUGO_PARAMS_API="https://admin.gallery.example.com/api/" # Use your manager domain
HUGO_PARAMS_CACHEDOMAIN="img.gallery.example.com" # Use your imageResizer domain
HUGO_PARAMS_TAGLINE="Site Tagline"
HUGO_PARAMS_THEMECOLOR="white" # white or grey
AWS_PROFILE=AWSProfile
AWS_REGION=AWSRegion
DOMAIN_NAME=gallery.example.com
CERTIFICATE_ARN=arn:aws:acm:us-east-1:XXXXXX:certificate/yyyyyyyy
HOSTED_ZONE_ID=HostedZoneId
HUGO_GOOGLEANALYTICS="UA-Tracking-Code"
WEBCLIENTID="webclientId" # aws_user_pools_web_client_id from albumsManager/src/aws-exports.js
```

* **WEBCLIENTID**: *aws_user_pools_web_client_id* from [Albums Manager]({{< relref "albums-manager.md" >}}).

{{% alert title="Multiple deployments" color="warning" %}}
If you are using several stacks in the same AWS account, you need to edit `.env` before deploying each of them.
{{% /alert %}}

## Deploy the stack
```bash
bash setup.sh
```

The CodeBuild project is run automatically once the stack is provisioned. If you don't want that, comment out the last line of *setup.sh*.

{{% alert title="S3 readiness" color="primary" %}}
If you deploy outside of *us-east-1*, your domain won't be accessible once the deploy is finished. You will need to wait up to one hour for the bucket redundancy to be ready. if you try to access you will be redirected to the Bucket webhosting endpoint. This is an [AWS issue](https://aws.amazon.com/premiumsupport/knowledge-center/s3-http-307-response/), nothing that can be done.
{{% /alert %}}

### What is created ?
* S3 Bucket
* Cloudfront Distribution
* CodeBuild Project
  * Triggered from EventBridge
