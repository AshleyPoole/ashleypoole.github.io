---
layout: post
title: Running Docker Containers On Windows Server 2016 (Quick Start Guide)
date: 2016-01-22 20:35
categories: [DevOps]
tags: [azure, docker]
---

So you love Docker and you've heard that Windows Server 2016 will support Docker containers but you've no clue where to start? Worry no more as I swiftly take you through the basics of building your first Windows based Docker container image containing IIS and create a new container using our container image to host a basic static HTML website.

For this guide, I'll be using a VM (virtual machine) running Windows Server 2016 which has been deployed using Microsoft's Azure cloud. You might want to run a VM locally but if not, head over to <a href="https://azure.microsoft.com/en-gb/documentation/articles/virtual-machines-windows-tutorial/" target="_blank">Azure's guide</a> on deploying a Windows Server VM to Azure, simply replace all references in the guide for "Windows Server 2012 R2 Datacenter" with "Windows Server 2016 Technical Preview 4".

## Installation
(1) Install Windows Server 2016 and log onto the server (See link above for deploying a Windows Server 2016 VM to Azure)  
(2) Open an elevated PowerShell prompt  
(3) Run the following commands which will download and run Microsoft's configuration script that will setup the Docker client, but more important  configure the server as a container host.
{% highlight powershell %}
# Download setup script
wget -uri http://aka.ms/setupcontainers -OutFile C:\SetupContainer.ps1

# Run setup script
C:\SetupContainer.ps1
{% endhighlight %}  
(4) After about a minute or two the server will reboot as part of the configuration stage. Simply log back in and your see a PowerShell window open and continue executing the script we started in the previous step.  
(5) During this next stage the script will enable container networking, create NAT rules, etc and then start installing the container OS image which will be downloaded from <a href="http://aka.ms/ContainerOSImage" taget="_blank">http://aka.ms/ContainerOSImage</a> automatically.  

*Note - the final step may take some time depending on your internet connection speed and VM resources. My Azure demo VM which is a Standard A1 (1 core, 1.75 GB memory) machine took around an hour and a half to complete this step.*

## GoGo Docker!
Before we dive into getting your first Windows based container up and running, I want to point out this technology is still in technical preview, so it may be rough around the edges and/or in some cases missing functionality.

With that out the way, I also wanted to mention one difference that you'll notice if you've ever run Docker on Windows before, in that the "docker-machine" command is no longer available. This is because Windows Server 2016's Docker implementation only supports Windows based containers due how the <a href="https://docs.docker.com/engine/introduction/understanding-docker/" taget="_blank">Docker isolation architecture</a> works. Maybe in the future we might see the "docker-machine" command return so that we can run a Linux VM on Windows in-which to host our Linux based containers as well, but let's not hold our breath just yet.

Let's quickly confirm that Docker has been set-up by checking the Docker version of the client and server components. This is done by running the "docker version" command from a PowerShell command prompt which should return similar output to the image below.

<a href="{{ "/img/2016/01/docker-version.png" | prepend: site.assetsbaseurl }}" taget="_blank"><img src="{{ "/img/2016/01/docker-version.png" | prepend: site.assetsbaseurl }}" alt="docker-version" width="600" height="212" class="alignnone size-full" /></a>

Now that we've confirmed Docker has successfully installed, let's go ahead and create a new image with IIS installed. We'll also then move on to creating a new container using our new image which we'll use to host our website that will be mounted from the hosts filesystem.

{% highlight powershell %}
# Create a container called demobase running Windows Server 2016 Core with an interactive session running PowerShell.
docker run -it --name demobase windowsservercore powershell

# After a short period, the container will be created and a PowerShell prompt from the container will be opened within your current windows.

# Let's now install IIS.
Install-WindowsFeature web-server

#After installation has finished, let's exist the containers PowerShell prompt.
exit
{% endhighlight %}

You'll now be returned to the PowerShell prompt on the Windows Server 2016 host where we will now turn the container into our own base image with IIS installed.

{% highlight powershell %}
# Committing our container called demobase to a new container base image called servercoreiis
docker commit basedemo servercoreiis

# After committing our new image, we can confirm our image has been created successfully by running the docker image command to list all base container images
docker images
{% endhighlight %}

<a href="{{ "/img/2016/01/docker-images.png" | prepend: site.assetsbaseurl }}" taget="_blank"><img src="{{ "/img/2016/01/docker-images.png" | prepend: site.assetsbaseurl }}" alt="docker-version" width="800" height="125" class="alignnone size-full" /></a>

Now that we have our custom container image, let's finally wrap up this quick start guide by creating a container using our custom image and mounting an example website's directory as a subdirectory under the IIS's default website directory. For this demo, I've copied one of my static HTML websites to "C:\docker-data\website" on my Windows Server 2016 host. Alternatively rather than mounting a directory from the host to the container, I could have instead copied the files from the host to the container using the "docker cp" command.

{% highlight powershell %}
# Create a Docker container called iisdemo from the servercoreiis image. Also mounting "C:\docker-data\website" from the host to the container and mapping host port 80 to port 80 on the container.
docker run -it -p 80:80 -v C:\docker-data\website:C:\inetpub\wwwroot\mywebsite --name iisdemo servercoreiis powershell

# Finally to test IIS is hosting our website, either run the below command or use a browser on the host and navigate to http://localhost/mywebsite/index.html
wget http://localhost/mywebsite/index.html
{% endhighlight %}

If all went well, you should now be looking at your webpage called index.html hosted using IIS from the Docker container we created. Hopefully I will get around to producing more Docker documentation soon :-) In the meantime let me know if you hit any issues with the guide.

Thanks!

-------------------------
**Update**  
You may also be interested in my later post on <a href="{% post_url 2016-02-08-publish-aspnet-5-windows-apps-docker-tools %}/" target="_blank">Publishing Applications To Windows Containers Using Visual Studio Docker Tools (Whirlwind Tour)</a>.
