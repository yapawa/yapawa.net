---
title: "Migrate from Koken"
linkTitle: "Migrate from Koken"
weight: 1
date: 2020-04-22
description: >
  Migrate from Koken CMS to Yapawa
---
[Koken](http://koken.me) is a PHP driven CMS for photographers.

Migrating your content from one CMS to another is never easy. That's the reason there is a tool for it.

## Koken To Yapawa

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/koken-to-yapawa">
  Github <i class="fab fa-github ml-2 "></i>
</a>

This tool will export all published albums using Koken's API, including hierarchy and store the corresponding metadata in Yapawa's tables.

All original images are downloaded into Yapawa's S3 bucket.

Cover images are assigned accordingly.

## Serverless

The whole process is serverless. The stack is orchestrated with [serverless.com](https://serverless.com/) and makes use of:
* [AWS Step Function](https://aws.amazon.com/step-functions/)
* [Lambda](https://aws.amazon.com/lambda/)

## Setup and Usage

Refer to the project's [Readme](https://github.com/yapawa/koken-to-yapawa/blob/master/README.md)

## Caveats
* Only public albums and public images are transferred.
* EXIF data stored outside the image aren't copied. Only EXIf data available in the original image will be used.
* Custom focus point of single images aren't copied.
* Text content from Koken isn't copied.
