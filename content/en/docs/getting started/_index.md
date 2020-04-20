---
title: "Getting Started"
linkTitle: "Getting Started"
weight: 1
date: 2020-04-20
description: >
  This page tells you how to get started with the Yapawa. What to prepare before installing each service.
---
## Prerequisites

* AWS Account
* IAM user with *Power user* rights
    {{% alert title="Needed Rights" color="warning" %}}
Not all services are needed. But there a enough services for which access is needed, that using a *Power User* simplifies the user configuration.
    {{% /alert %}}
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
* [AWS CLI named profile](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html)
* [nvm](https://github.com/nvm-sh/nvm/blob/master/README.md)
* NodeJS: `nvm install node`

## Domains
The domains need to be hosted in [Route53](https://aws.amazon.com/route53/). Cloudformation will automatically create the relevant Aliases.

You need to define 3 domains. One for each service. To make it easier, it is recommended to use sub-domains. But you don't have too.

As example:
* Albums Manager: admin.gallery.example.com
* Image Resizer: img.gallery.example.com
* Public Site: gallery.example.com

### Hosted Zone ID
Retrieve the *HostedZoneId* for each domain. You will need to paste it into the configuration files of each service.

```bash
HOSTEDZONEID=$(aws --profile myprofile route53 list-hosted-zones --query 'HostedZones[?Name==`example.com.`].Id' --output text | cut -d'/' -f3)
```
{{% alert title="AWS Console" color="primary" %}}
You can also retrieve the hostedZoneId from the [AWS Console](https://console.aws.amazon.com/route53/home?#hosted-zones:)
{{% /alert %}}
{{% alert title="Multiple domains" color="warning" %}}
If you decided to use different base domains for your services, you will need to retrieve each HostedZoneId.
{{% /alert %}}

## ACM Certificates
[ACM Certificates](https://aws.amazon.com/acm/) for [Cloudfront](https://aws.amazon.com/cloudfront/) need to be generated in **us-east-1**.
And since [Cloudformation](https://aws.amazon.com/cloudformation/) isn't able to easily do cross region stacks,
we will create the certificates manually and only give their ARN as reference to our stacks.

[Open ACM](https://console.aws.amazon.com/acm/home?region=us-east-1#/) in **us-east-1**, generate a single certificate for *gallery.example.com* as primary domain and *\*.gallery.example.com* as alternate domain.
Use DNS as validation method, click the button to create the entries in Route53.

Keep the ARN (arn:aws:acm:us-east-1:*AccountId*:certificate/*certificateId*) somewhere.

Wait for the certificate to be valid.
{{% alert title="Multiple domains" color="warning" %}}
If you decided to use different base domains for your services, you will need to create multiples certificates and keep each of the ARN's.
{{% /alert %}}

## Amplify
If you haven't already setup [Amplify](https://aws-amplify.github.io/):
```bash
npm install -g @aws-amplify/cli
amplify configure
```

## AWS Region
The services don't need to be in the same region. But it is highly recommended, mainly for *imageResize* and *publicsite* to be in the same region.

## Next
* [Setup Albums Manager]({{< relref "albums-manager.md" >}})
* [Setup Image Resize]({{< relref "image-resize.md" >}})
* [Setup Public Site]({{< relref "public-site.md" >}})
