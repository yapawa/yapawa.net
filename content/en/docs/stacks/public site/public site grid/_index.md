---
title: "Public Site Grid"
linkTitle: "Public Site Grid"
weight: 3
date: 2020-04-22
description: >
  This is the default layout. It is very simplistic and minimalist. The goal is to be lightweight and fast rendering.
---
## Infrastructure Overview

{{< imgproc publicSiteGrid Resize "400x" "Infrastructure Overview" >}}
{{< /imgproc >}}

<a class="btn btn-sm btn-secondary mr-3 mb-4" href="https://github.com/yapawa/imageResize">
  Github <i class="fab fa-github ml-2 "></i>
</a>

*publicsite-grid* is the default and very simplistic layout to display your images to the public.

## Stack

The site uses [Hugo](https://gohugo.io/), a static site generator, to pre-build all the html pages.

The images are served using [imageResize]({{< relref "../../image resize/_index.md">}}). The template needs to be aware of the path structure needed.

## Build

The stack provisions a [CodeBuild](https://aws.amazon.com/codebuild/) stack, which will launch upon an [EventBridge](https://aws.amazon.com/eventbridge/) message sent by the user in [albumsManager]({{< relref "../../albums manager/_index.md" >}}).

## Color Scheme

The layout comes with 2 color schemes: white backround (default) or grey background. This value is defined in `config.toml`.

## Albums and Images
A NodeJS script ([makeContent.js](https://github.com/yapawa/publicsite-grid/blob/master/makeContent.js)) will read the /tree endpoint of the public API.

Using this data, it will create a *page* for each album, respecting the hierarchy and ordering.

For collections, it will create a folder. For albums, it will fetch the album content from the public API and populate the Markdown content.

Hugo will then render the site to HTML content.

## Masonry Grid

No javascript is used to generate the grid layout, only pure CSS. Making use of [CSS Grid Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout).

## Responsive

The layout is totally responsive. It uses the available real estate on Dekstop and Mobile.

[Webp](https://developers.google.com/speed/webp) is used when possible. And the various breakpoints related to screen density allow a crystal clear display of each image.

## Lightbox

[PhotoSwipe](https://photoswipe.com/) is used to provide an image lightbox with swipe capabilities.

## AMP

The layout produces 2 different outputs: one classic and one in [AMP](https://amp.dev/). Both outputs look very similar since the classic version is already minimal and optimized.

## SEO

LD+JSON and various OpenGraph metadata tags are used to improve SEO.

## Adding more Hugo themes

*Grid* is a theme under a basic Hugo setup. Any other theme using Hugo could be added in this repo. PR welcome.

The them to use can be defined in `config.toml`.

{{% alert title="Repo naming" color="secondary" %}}
Obviously the repo suffix "-grid" wouldn't make sense anymore if there are more themes available.
{{% /alert %}}
