---
layout: post
title: Managing JAMS with Powershell
date: 2014-04-07 20:43
categories: [devops]
tags: [powershell]
---
List of command examples for managing JAMS using Powershell. Once you have installed the JAMS client locally, you can import the module using:

{% highlight powershell %}import-module JAMS{% endhighlight %}

<h2>Job History</h2>
<strong>Get failed jobs within the last 24 hours</strong>
{% highlight powershell %}Get-JAMSHistory -Server JAMSServer -Status error{% endhighlight %}

<strong>Get the history of failed jobs within the last 24 hours that are also in the PROD folder</strong>
{% highlight powershell %}Get-JAMSHistory -Server JAMSServer -Folder prod -Status error{% endhighlight %}

<strong>Get the history of jobs within the last 24 hours who's name begins with FTP</strong>
{% highlight powershell %}Get-JAMSHistory -Server JAMSServer -Name FTP* {% endhighlight %}

<h2>Job Management</h2>
<strong>Pause a queue called PROD</strong>
{% highlight powershell %}Stop-JAMSQueue -Server JAMSServer -Name PROD {% endhighlight %}

<strong>Stop all queues on the server - Executing jobs finish their current execution</strong>
{% highlight powershell %}Get-JAMSQueue -Server JAMSServer | Stop-JAMSQueue {% endhighlight %}

<strong>Start all queues on the server</strong>
{% highlight powershell %}Get-JAMSQueue -Server JAMSServer | Start-JAMSQueue {% endhighlight %}

<strong> Get list of all executing jobs</strong>
{% highlight powershell %}Get-JAMSEntry -Server JAMSServer -State executing {% endhighlight %}

<strong>Pause a job - I think this will only pause it until it's next schedule kicks off</strong>
{% highlight powershell %}Suspend-JAMSEntry -Server JAMSServer -Name MyImportantJob {% endhighlight %}

<strong>Resume a job</strong>
{% highlight powershell %}Resume-JAMSEntry -Server JAMSServer -Name MyImportantJob {% endhighlight %}
