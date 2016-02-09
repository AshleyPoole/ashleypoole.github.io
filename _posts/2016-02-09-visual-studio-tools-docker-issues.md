---
layout: post
title: Publishing Applications To Windows Containers Using Visual Studio Docker Tools (Common Issues)
date: 2016-02-09 18:30
categories: [DevOps]
tags: [docker]
---
In-short this post is part two following on from my previous post entitled <a href="{% post_url 2016-02-08-publish-aspnet-5-windows-apps-docker-tools %}/" target="_blank">Publishing Applications To Windows Containers Using Visual Studio Docker Tools (Whirlwind Tour)</a> which showed you a whirlwind tour of how to publish an application to a Windows based Docker container from within Visual Studio. This post is intended for anyone may or may not have followed that demo but has experienced  issues with Docker and <a href="https://visualstudiogallery.msdn.microsoft.com/0f5b2caa-ea00-41c8-b8a2-058c7da0b3e4" target="_blank">Visual Studio Tools for Docker</a>.

*This FAQ document is based on errors I encountered using a newly created greenfield ASP.NET 5 / ASP.NET Core demo project build using ASP.NET 5 RC1, Visual Studio 2015 Update 1 and Visual Studio Tools for Docker - December Preview. If you're interested in the demo website and Docker configuration, then it can be found on <a href="https://github.com/AshleyPoole/KaleLovers" target="_blank">my GitHub account</a>.*

*As ASP.NET 5 / ASP.NET Core and Windows Server 2016 are both in release candidate or technical preview at the time of writing this post, I just want to emphasize that the relevance of this post might diminish quite fast after publishing.*

-------------------------

### Unable To Locate Runtime Error ###
{% highlight bash %}
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(152,5): Error : Unable to locate runtime 'dnx-coreclr-win-x64.1.0.0-rc1-final'
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(152,5): Error : Locations probed:
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(152,5): Error : dnx-coreclr-win-x64.1.0.0-rc1-final
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(152,5): Error : C:\Users\Ash\.dnx\runtimes\dnx-coreclr-win-x64.1.0.0-rc1-final
{% endhighlight %}

The resolution is to install the missing DNX runtime. In my case (based on the error message), the installation command for the missing runtime using the DNVM (DotNet Version Manager) was:  
**dnvm install 1.0.0-rc1-final -r coreclr -a x64**

-------------------------

### Windows vs Linux Dockerfile's Errors ###
I ran into two different errors which were both related to the Dockerfile Visual Studio Tools for Docker had generated which are both listed below. The common issue for both of errors is because the Dockerfile generated currently defaults to values suitable to run a Linux based container and not a Windows based container. This means when you try to push this container to a Windows container host, you get some strange (but explainable) errors.

#### Image microsoft/aspnet:1.0.0-* Not Found Error ####
{% highlight bash %}
Sending build context to Docker daemon 206.8 MB
Sending build context to Docker daemon 206.8 MB
Step 1 : FROM microsoft/aspnet:1.0.0-rc1-update1-coreclr
Pulling repository docker.io/microsoft/aspnet
Error: image microsoft/aspnet:1.0.0-rc1-update1-coreclr not found
Please visit http://go.microsoft.com/fwlink/?LinkID=529706 for troubleshooting guide.
{% endhighlight %}


#### Container Command Not Found Or Does Not Exist Error ####
{% highlight bash %}
VERBOSE: Removing intermediate container 1b6cba30cba7
VERBOSE: Step 4 : ENTRYPOINT ./web
VERBOSE:  ---> Running in dd76bfb2dd00
VERBOSE:  ---> 57a241cd2da3
VERBOSE: Removing intermediate container dd76bfb2dd00
VERBOSE: Successfully built 57a241cd2da3
The Docker image "kalelovers" was created successfully.
VERBOSE: Starting Docker container: kalelovers
Executing command [docker --tlsverify --tlscacert="C:\Users\Ash\.docker\demo04\ca.pem" --tlscert="C:\Users\Ash\.docker\demo04\cert.pem" --tlskey="C:\Users\Ash\.docker\demo04\key.pem" -H tcp://demo04.northeurope.cloudapp.azure.com:2376 run -t -d -p 80:80 -e "server.urls=http://*:80" --name 80_80 kalelovers]
VERBOSE: ad17de57213b42192c520b20f0012d4bac4910373bbb0f7ddade12368c301934
VERBOSE: Error response from daemon: Container command not found or does not exist.
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(386,5): Error : An error occured during publish.
The command [docker --tlsverify --tlscacert="C:\Users\Ash\.docker\demo04\ca.pem" --tlscert="C:\Users\Ash\.docker\demo04\cert.pem" --tlskey="C:\Users\Ash\.docker\demo04\key.pem" -H tcp://demo04.northeurope.cloudapp.azure.com:2376 run -t -d -p 80:80 -e "server.urls=http://*:80" --name 80_80 kalelovers] exited with code [1]: ad17de57213b42192c520b20f0012d4bac4910373bbb0f7ddade12368c301934
Error response from daemon: Container command not found or does not exist.
Please visit http://go.microsoft.com/fwlink/?LinkID=529706 for troubleshooting guide.
{% endhighlight %}


The resolution is to manually update your Dockerfile to update the containers image name with that of a Windows based container image as well as updating the entrypoint command. Below is a Linux Vs Windows example showing the basic differences.

**Linux Dockerfile**
{% highlight bash %}
FROM microsoft/aspnet:1.0.0-rc1-final
ADD . /app
WORKDIR /app/approot
ENTRYPOINT ["./web"]
{% endhighlight %}

**Windows Dockerfile**
{% highlight bash %}
FROM windowsservercore
ADD . /app
WORKDIR /app/approot
ENTRYPOINT ["cmd.exe", "/k", "web.cmd"]
{% endhighlight %}

-------------------------

### AspnetPublishHandler With Name Custom Was Not Found ###
{% highlight bash %}
Publishing with publish method [Custom]
C:\Program Files (x86)\MSBuild\Microsoft\VisualStudio\v14.0\Web\Microsoft.DNX.Publishing.targets(386,5): Error : An error occurred during publish.
AspnetPublishHandler with name "Custom" was not found
{% endhighlight %}

I found the resolution to this error was to delete the 'publishProfile' folder from within project and try publishing again.

-------------------------

If you've got any comments or other issues you'd like be to add then drop me a note below in the comments!
