---
title: "Welcome to Yapawa"
linkTitle: "Documentation"
menu:
  main:
    weight: 20
---
Welcome to the Yapawa user guide. This guide will help you configure and deploy the core services.

It will also explain you how to manage your albums and images and deploy them to your public site.

You will also be able to find help on how to build your own frontend site.

## What is Yapawa ?
Yapawa is an acronym for "Yet Another Photo Album Web App". As the name implies, it is just another photo gallery website.

The features specific to Yapawa:
* Serverless: It uses specific serverless services from [AWS](https://aws.amazon.com)
* Separated concerns by having different applications handling different parts:
  * CMS
  * Image resizing
  * Published public site
* Infinite nested levels of albums
* Any image can be used as album cover

## Is Yapawa for me ?
You need to have some basic technical skills, Yapawa's installation isn't a click-next type of install.

You will also need:

* An AWS account with a user with administrator rights.
* Basic knowledge of AWS and it's services.
* An [AWS CLI](https://aws.amazon.com/cli/) configured with [profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html).
* Basic knowledge of terminal commands.
* Ability to install npm packages globally on your development machine.

Yapawa allows you to only create albums and images. Videos aren't supported. You can only publish your content worldwide.
If you want to share private albums with selected persons, Yapawa isn't for you.

All persons with access to the CMS, will have management access to all albums and images. If you want to have multiples administrators, each managing their own pictures, Yapawa isn't for you.

## Ready to get Started ?
Follow the [Getting Started]({{< relref "getting started/_index.md" >}}) guide or read the detailed description for each service.
