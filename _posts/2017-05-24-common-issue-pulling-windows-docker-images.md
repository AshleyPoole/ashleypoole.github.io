---
layout: post
title: Common Issue Pulling Docker Images On Windows (Antivirus)
date: 2017-05-24 23:10
categories: [devops]
tags: [docker]
---

Running the Docker engine natively on Windows operating systems, as well as orchestrating containers running Windows Server 2016 is still fairly new and shiny, and with that comes new issues; some documented more than others. One such issue which isn't documented very well except for [GitHub issues](https://github.com/Microsoft/Virtualization-Documentation/issues/355){:target="_blank"} that beginners are facing is failures when pulling images from their Docker registry - most commonly the [Docker Hub registry](https://hub.docker.com){:target="_blank"}.

For those who don't know, Docker images are built up of various layers and while this post ins't going to go into what layers are exactly, I recommend you go and read [Docker's documentation on layers](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/){:target="_blank"} if you are unsure.

The exact error I wanted to share in this blog post occurs after the various layers of a Docker image have been pulled down from the registry and begin to extract. The error typically occurs once it finishes extracting the final byte of the first layer of the image being pulled. The error can be see in the below error text and screenshot.

>failed to register layer: re-exec error: exit status 1: output: ProcessBaseLayer C:\ProgramData\Docker\windowsfilter\{layer ID is shown here}: The I/O operation has been aborted because of either a thread exit or an application request.

<img src="{{ "/img/2017/docker-extract-error.png" | prepend: site.assetsbaseurl }}" alt="docker pull error" width="864" height="225" style="margin:0px auto;display:block"/>

The root cause of this issue is that most antivirus packages take a lock on the temporary file the extraction process has written the image layer to. This which means the Docker pull task fails when it's trying to process the image layer after extraction.

A common solution being used is to exclude the ProgramData path for Docker from antivirus scanning. This commonly means adding an exclusion for "C:\ProgramData\Docker\" and all sub directories though check on your IT departments security policy on this first if you work for a corporation before adding exclusions.

After adding this exclusion, you should hopefully no longer see issues related to extracting when pulling Docker images down to your Windows operating system based Docker host.
