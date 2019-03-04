---
layout: default
title: GitHub Overview
---


# GitHub Overview

This page shall give an overview of my public projects.

Please also note [this list of my pull requests and issues on other projects](https://github.com/search?q=author%3Aznegva&type=Issues).

&nbsp;

## resources-generator
[![Build Status](https://travis-ci.com/znegva/resources-generator.svg?branch=master)](https://travis-ci.com/znegva/resources-generator)
[![Codecov](https://codecov.io/gh/znegva/resources-generator/branch/master/graph/badge.svg)](https://codecov.io/gh/znegva/resources-generator)
[![npm version](https://badge.fury.io/js/resources-generator.svg)](https://badge.fury.io/js/resources-generator)

The key reason to start this project was that I couldn't find a suitable tool for creating graphics for a Cordova project that could exactly meet all my requirements.
It should be easy to configure and support the creation of NinePatch images for the Android platform.

Currently you can use it either via command line or within your own scripts.
If you use it in your own scripts, you are free to choose the exact specifications you want to use for the creation.
As a first starting point you can use the typical specifications for icons and splashscreen of the Android and iOS platforms, which are also included.

[gm](http://aheckmann.github.io/gm/) and [imageMagick](http://www.imagemagick.org/) are used for image creation.

Later, with the help of this project, I decided to become familiar with other tools as well.
To try out the release of the script itself via npm, continuous integration via Travis CI and the release of code coverage data via Codecov.

| Tools                      | Links                                                                      |
| -------------------------- | -------------------------------------------------------------------------- |
| TypeScript                 | [Github project repository](https://github.com/znegva/resources-generator) |
| Mocha and Chai for testing |                                                                            |
| npm                        | [npm project page](https://www.npmjs.com/package/resources-generator)      |
| Travis CI                  | [Travis CI project page](https://travis-ci.com/znegva/resources-generator) |
| Codecov                    | [Codecov project page](https://codecov.io/gh/znegva/resources-generator)   |


&nbsp;

## In-App-Purchase validation service
<i class="fas fa-server"></i>
<i class="fab fa-node-js"></i>

For a number of Cordova projects, I use the [Cordova Plugin Purchase](https://github.com/j3k0/cordova-plugin-purchase) plugin to perform in-app purchases.

In order to be able to use the plugin to its full extent, an additional service is required that is operated independently of the actual app and takes care of the validation of purchases from the Apple App Store as well as the Google Play Store.

Such a service was realized with the help of this [node.js module for in-app purchases](https://github.com/voltrue2/in-app-purchase) and [Express](https://expressjs.com/) and is operated by me on a separate server.

The script is hosted on GitHub.  
But to be able to perform the tests login credentials to App Stores and some receipt data is needed - these are not provided via Github.

| Tools                      | Links                                                                         |
| -------------------------- | ----------------------------------------------------------------------------- |
| Node.js, Express           | [Github project repository](https://github.com/znegva/iap_validation_service) |
| Mocha and Chai for testing |                                                                               |

&nbsp;

## gurzr (Bolt CMS theme)

<i class="fab fa-php"></i>
<i class="fab fa-html5"></i>
<i class="fab fa-sass"></i>
<i class="fab fa-css3"></i>
<i class="fab fa-js"></i> 

A single column theme for [Bolt CMS](//bolt.cm), with support for image galleries, GPX tracks, optional password protection for posts and some individual customisation options.

| Tools                | Links                                                                        |
| -------------------- | ---------------------------------------------------------------------------- |
| Twig template engine | [Github project repository](https://github.com/znegva/gurzr)                 |
| Sass                 | [Bolt CMS market entry](https://market.bolt.cm/view/znegva/bolt-theme-gurzr) |
| some JavaScript      |                                                                              |

&nbsp;

## Hyde for Bolt (Bolt CMS theme)
<i class="fab fa-php"></i>
<i class="fab fa-html5"></i>
<i class="fab fa-css3"></i>
<i class="fab fa-js"></i>

Port of Hyde theme for [Bolt CMS](//bolt.cm).  
Hyde is a brazen two-column theme that pairs a prominent sidebar with uncomplicated content. It's based on Poole, the Jekyll butler. Both created by Mark Otto.

| Tools                | Links                                                                       |
| -------------------- | --------------------------------------------------------------------------- |
| Twig template engine | [Github project repository](https://github.com/znegva/hyde-for-bolt)        |
| CSS                  | [Bolt CMS market entry](https://market.bolt.cm/view/znegva/bolt-theme-hyde) |
| some JavaScript      |                                                                             |


&nbsp;

## znegva.github.io (this site)
<i class="fab fa-github"></i>
<i class="fab fa-markdown"></i>

The website you are watching right now.
I have always documented my technology-related activities where necessary - also to be able to reproduce certain decisions, processes or settings later.
Topics that might be of use to others can be published.

| Tools                | Links                                                                   |
| -------------------- | ----------------------------------------------------------------------- |
| Markdown             | [Github project repository](https://github.com/znegva/znegva.github.io) |
| Jekyll, GitHub Pages |                                                                         |

&nbsp;

## zerodistraction theme
<i class="fab fa-python"></i>
<i class="fab fa-html5"></i>
<i class="fab fa-css3"></i>
<i class="fab fa-js"></i>

I also created the [zerodistraction theme](https://github.com/znegva/zerodistraction-theme) for [Pelican](https://blog.getpelican.com/), a static site generator written in Python.

&nbsp;

## Oscar-Ghost-mod theme
<i class="fab fa-node-js"></i>
<i class="fab fa-html5"></i>
<i class="fab fa-css3"></i>

I also [modified the Oscar theme](https://github.com/znegva/oscar-ghost-mod) for [Ghost](https://ghost.org/), a blogging platform for Node.js.


<!--
## Appreciations

- <https://github.com/2016rshah/githubchart-api>
  - <img src="http://ghchart.rshah.org/267CB9/znegva" alt="znegva's Github chart" />

-->
