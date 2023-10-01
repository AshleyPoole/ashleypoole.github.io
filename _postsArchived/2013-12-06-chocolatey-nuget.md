---
layout: post
title: Chocolatey NuGet
date: 2013-12-06 20:18
categories: [devops, knowledge-base]
tags: [powershell]
---
Speak to any developer who uses Visual Studio and you're be hard pressed to find someone who hasn't heard if not uses <a title="NuGet" href="http://www.nuget.org/" target="_blank">Nuget</a> on a regular basis. For those who don't know, NuGet is a package manager which when commanded will take the selected package from the online gallery and download the library files and updates your solution to include them. Easy right?

But what if you could use this interface and process you've become familiar with to install those applications and tools that you need for developing or your day to day IT job? What if you could setup a new workstation with your application requirements in one easy step? Here comes <a title="Chocolatey" href="http://chocolatey.org" target="_blank">Chocolatey</a>!

Chocolatey is referred to as a machine level package manager that utilises the NuGet infrastructure and is sometimes called the apt-get of Windows.

Now let's stop talking about Chocolatey and get stuck in! To install Chocolatey let's fire up Powershell and run the below. Note - This requires the execution policy to be set to unrestricted, if you're unsure how to do this, checkout this <a title="Powershell Execution Policy" href="http://technet.microsoft.com/en-us/library/ee176961.aspx" target="_blank">Technet article.</a>

{% highlight powershell %}
iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))&amp;amp;quot;
{% endhighlight %}

An alternative way to install Chocolatey and set the required execution policy settings are to the run the below in CMD.

{% highlight powershell %}
@powershell -NoProfile -ExecutionPolicy unrestricted -Command &amp;amp;quot;iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
{% endhighlight %}

Now time to look at getting those applications and tools installed. At the time of writing this, the <a title="chocolatey gallery" href="http://chocolatey.org/packages" target="_blank">Chocolatey Gallery</a> has 1409 unique packages available for install so there's plenty of choice. I personally use 5 out of the top 10 current popular packages including Notepad++, GIT and VLC.

To install VLC, simply run the below command from Powershell (because who uses still uses CMD right?).

{% highlight powershell %}
cinst vlc
{% endhighlight %}

And that ladies and gentlemen is VLC installed. Easy! Now simply go through their online gallery of packages and knock up a simple script to install all your required items. Setting up a new workstation with your personal requirements just got a lot easier.

For more technical information see their Wiki hosted on Github - https://github.com/chocolatey/chocolatey/wiki
Chocolatey Package Gallery - http://chocolatey.org/
