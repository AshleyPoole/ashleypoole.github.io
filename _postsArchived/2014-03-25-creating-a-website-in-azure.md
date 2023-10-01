---
layout: post
title: Creating A Website In Azure
date: 2014-03-25 20:11
author: ashley@ashleypoole.co.uk
comments: true
categories: [devops, server-admin]
tags: [azure]
---
<h2>Azure, what?</h2>
For those who don't know (Where have you been hiding these last 4 years?) Windows Azure is Microsofts cloud computing platforms which provides both PaaS* and IaaS*. A greatly unused benefit of MSDN subscriptions is a free allocation of Azure credits who's value will depends on your subscription level. At the time of writing this, the 'Visual Studio Professional with MSDN' subscription level comes with £35 British pounds or $50 US dollars worth of Azure credits.

I'm not intending to cover in any detail what Azure is, so if you would like more in-depth details of what Azure is or what it can offer head over to here for a quick read first - <a title="What Is Azure" href="http://www.windowsazure.com/en-us/overview/what-is-windows-azure/" target="_blank">http://www.windowsazure.com/en-us/overview/what-is-windows-azure</a>.
<h2>Getting HelloWorld Set-up</h2>
For this series I will assume you have already got an Azure account though if this isn't the case head over to <a title="Azure Free Trial" href="http://www.windowsazure.com/en-us/pricing/free-trial" target="_blank">http://www.windowsazure.com/en-us/pricing/free-trial</a> where you can sign up for a free trial.

To manage your Azure resources you need to log into their management portal which can be accessed at <a title="Manage Azure" href="https://manage.windowsazure.com" target="_blank">https://manage.windowsazure.com</a>. Upon logging in you are shown a list of all your items whether that be Websites, Virtual Machines, Databases, etc. You are also given an overview of your credit status which when clicked will show your remaining credits and days remaining until your credits expire.

<p style="text-align: center;"><a href="{{ "/img/2014/03/AzureManagementHome.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1059" src="{{ "/img/2014/03/AzureManagementHome.jpg" | prepend: site.assetsbaseurl }}" alt="Azure Management Home Screenshot" width="838" height="804" /></a></p>

Now we're logged in let's create our first website in Azure. This is achieved by clicking the 'Web Sites'  tab, then click 'Create A Website'. This will then by default present you with the 'Quick Create' option. You will be asked to provide a URL for your website as well as which region the website will be hosted. The Url by default will use the azurewebsites.net as the root domain name to which your Url (Subdomain) will be created under. This can be changed latter.

<a href="{{ "/img/2014/03/AzureWebsites1.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter" src="{{ "/img/2014/03/AzureWebsites1.jpg" | prepend: site.assetsbaseurl }}" alt="Azure Website Management" width="838" height="645" /></a>

After a few seconds your website will be created and brought online. From this screen you can click your website instance name to bring up the configuration area for this website. As you may have noticed, the mode is currently set to Free. Having the website mode set to free restricts the features available to you such as custom domain names which you will most likely require for a public facing website as well as other features.

<p style="text-align: center;"><a href="{{ "/img/2014/03/AzureWebsiteCreated.png" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1065" src="{{ "/img/2014/03/AzureWebsiteCreated.png" | prepend: site.assetsbaseurl }}" alt="Azure Website Created" width="838" height="136" /></a></p>

Upon clicking on the instance name, you are taken to the quick start screen for the website instance where you are prompted to setup key items for your instance. For now, let's get the credentials setup so you can deploy your website to the instance by choosing the 'Set up deployment credentials' option.

<p style="text-align: center;"><a href="{{ "/img/2014/03/AzureQuickStart.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1070" src="{{ "/img/2014/03/AzureQuickStart.jpg" | prepend: site.assetsbaseurl }}" alt="Azure Website Quick Start" width="838" height="518" /></a></p>

A username and password combination will be required from you that will become your deployment credentials.

<p style="text-align: center;"><a href="{{ "/img/2014/03/AzureNewDeploymentCreds.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1071" src="{{ "/img/2014/03/AzureNewDeploymentCreds.jpg" | prepend: site.assetsbaseurl }}" alt="Azure New Deployment Creds" width="553" height="586" /></a></p>

Now from the Dashboard page, locate your deployment username and ftp host. The deployment username is usually a combination of the username you chose previously appended to the website instance name. Using your favourite FTP client, make a connect to the FTP server listed using the deployment user and password you choose, then upload your website.

<a href="{{ "/img/2014/03/AzureDashboard.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1072" src="{{ "/img/2014/03/AzureDashboard.jpg" | prepend: site.assetsbaseurl }}" alt="Azure Dashboard Snippet" width="378" height="458" /></a>

<p style="text-align: left;">Once you have uploaded your site, goto your site Url. You should now see the website you have just uploaded. In my case this is my basic, hand written html HelloWorld page.</p>

<p style="text-align: center;"><a href="{{ "/img/2014/03/AzureHelloWorldUploaded.jpg" | prepend: site.assetsbaseurl }}"><img class="aligncenter  wp-image-1073" src="{{ "/img/2014/03/AzureHelloWorldUploaded.jpg" | prepend: site.assetsbaseurl }}" alt="HelloWorld Website Running" width="896" height="186" /></a></p>

<p style="text-align: left;">Congratulations, you've just setup and uploaded your first website to the Azure cloud! In future posts of this series I intend to cover the following sections:</p>

<ul>
	<li>Deployment trigger by source control (GIT / GITHUB)</li>
	<li>Upgrading from Free mode to Standard
<ul>
	<li>Using custom domain name for the website</li>
</ul>
</li>
	<li>Configuring the PHP and .NET Framework version</li>
	<li>Configuring other aspects such as virtual directories, logging, default documents, etc</li>
	<li>Monitoring</li>
</ul>

<h5>*Terminology explained</h5>
<ul>
	<li>PaaS - <a title="Platform as a service" href="http://en.wikipedia.org/wiki/Platform_as_a_service" target="_blank">Platform as a service</a></li>
	<li>Iaas - <a title="infrastructure as a service" href="http://en.wikipedia.org/wiki/Infrastructure_as_a_service" target="_blank">Infrastructure as a service </a></li>
</ul>
