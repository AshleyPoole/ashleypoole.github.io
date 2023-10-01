---
layout: post
title: PowerShell Just Enough Admin
date: 2014-06-22 17:27
categories: [devops]
tags: [powershell]
---
With everyone being on the PowerShell Desired State Configuration (DSC) hype including myself, it's been easy to let items like Just Enough Admin (JEA) pass us by without notice. That's why I decided to watch a <a title="JEA Session Video" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362#fbid=" target="_blank">session on JEA</a> from <a title="TechEd North America 2014" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014" target="_blank">TechEd North America 2014</a> and create this post from my own experiences as well.

Note - You must run any code examples within an administrative PowerShell prompt.
<h2>What Is JEA?</h2>
JEA is as the names says, is about providing just enough administrative access to accomplish a task or role without having to grant unnecessary access like making someone a local administrator just so they can restart a few services here and there. Think back to Edward Snowden days.

As IT professionals we are fast becoming the attack surface into our systems as well as the traditional surface of the actual systems themselves. Why you ask? If a hacker can manage to get your credentials, they are quickly able to spider out into many systems without much effort compared to the effort needed to spider out if they only had control of one system.

<h2>Concepts Of JEA</h2>
The concepts to JEA framework are as follows:
<ul>
	<li>JeaToolKit - Defines a set of commands / cmdlets which are authorised in order to preform a set of activities. This allows for very granular control is which a key principle to the JEA philosophy.</li>
	<li>JeaEndPoint - Management end point where authorised users are provided access to JeaToolKits which run as a JeaEndPointAccount.</li>
	<li>JeaEndPointAccount - The account in which the end point will execute commands as.</li>
</ul>

<h2>Where Can You Find The JEA Module?</h2>
The JEA module is available for download as part of the <a title="DSC Resource Kit" href="http://gallery.technet.microsoft.com/DSC-Resource-Kit-All-c449312d" target="_blank">DSC Resource Kit</a> which at the time of writing this is in iteration 4. Currently JEA is in an experiment state so when you come to import the module you will find it's name is prefixed with an x so is called 'xJEA'. JEA also requires Windows 8.1 or Server 2012 R2 as well as the <a title="Windows Management Framework 5.0 Preview" href="http://www.microsoft.com/en-us/download/details.aspx?id=42936" target="_blank">Windows Management Framework 5.0 Preview (May 2014)</a>.

<h2>Authoring And Applying A JeaToolKit</h2>
JEA is configured and applied using PowerShell DSC which significantly reduces the amount of manual configuration needed in order to start using and deploying JEA to your environment. Using DSC also reduces the possibility of configuration diff. For articles on PowerShell DSC that I have authored, <a title="Ashley Poole DSC Articles" href="http://www.ashleypoole.co.uk/tag/dsc/" target="_blank">click here</a>.

Authoring a JeaToolKit is very simply and if you are familiar with DSC then there isn't much extra that you will need to learn. Once the DSC resource kit is downloaded and installed, as well the other dependencies of JEA, you will then be ready to start using JEA. Alternatively to manually downloading the whole DSC resource kit, you can use the new Install-Module cmdlet available in the PowerShellGet (part of Windows Management Framework 5.0) in order to get the xJea module. Below are the commands to install JEA module using PowerShellGet should you prefer this new method.

{% highlight powershell %}
Find-Module –Name xJEA
# Confirm you see the JEA module listed then execute the below command
Install-Module -Name xJEA
{% endhighlight %}

Now that we have the JEA module installed, we have to configure before we can start authoring a JeaToolKit which is Windows Remote Management (WinRM). This can be easily configured using the below command:

{% highlight powershell %}
WinRM QuickConfig
# Confirm you agree to the listed changes list above then type Y then hit enter
{% endhighlight %}

Now we will examine one of the JEA demo scripts which will show us the structure needed in order to define a JeaToolKit and JeaEndPoint.

{% highlight powershell %}
cd "C:Program FilesWindowsPowerShellModulesxJeaExamples"
ISE .demo1.ps1
{% endhighlight %}

In the demo script, a DSC configuration block called 'Demo1' is declared which then contains two child elements which are the JeaToolKit called 'Process' and the JeaEndPoint called 'Demo1EP'.

Examining the JeaToolKit called 'Process', the 'CommandSpecs' property specifies the cmdlets along with filters that are configured for use. In this example, an IT professional using the 'Process' tool kit would be allowed to get process information (Get-Process), get service information (Get-Service), stop process who's process name is either calc or notepad (Stop-Process,Name,calc;notepad) and lastly also restart services who's name begins with the letter 'A' (Restart-Service,Name,,^A).

Now examining the JeaEndPoint called 'Demo1EP', We can see the 'ToolKit' name has been set to 'Process' which is the name of the JeaToolKit created earlier in the configuration block. The 'DependsOn' property is then used to ensure the JeaToolKit is created first before trying to create the JeaEndPoint. Should the JeaToolKit fail to be created or is missing, then DSC will then skip creation of the JeaEndPoint as it's dependencies hasn't been fulfilled.

The last two commands then compile the .MOF files and execute Demo1 configuration on Localhost.

<a href="{{ "/img/2014/06/JeaDemo1Summary.png" | prepend: site.assetsbaseurl }}"><img class="aligncenter size-full wp-image-1266" src="{{ "/img/2014/06/JeaDemo1Summary.png" | prepend: site.assetsbaseurl }}" alt="Jea Demo1 Script" width="771" height="438" /></a>

<h2>Connecting To An JeaEndPoint</h2>
Connecting to an JeaEndPoint is very similar to how you would start a normal remote PSSession. For JEA however, you also have to specify a parameter called ConfigurationName which is the name of the JeaEndPoint you wish to connect to. It is important to get the correct JeaEndPoint name, as different JeaEndPoints will likely give you different permissions or even deny you access if you aren't allow to access that JeaEndPoint.

{% highlight powershell %}
Enter-PSSession -ComputerName Server01 -ConfigurationName Demo1EP
{% endhighlight %}

Once you are connected to an JeaEndPoint, running 'Get-Command' will only show the approved list of cmdlets that have been defined and configured within the JeaToolKit / JeaEndPoint that you have connected to.

Like any normal PSSession, you also have to exit the session once you are complete.

{% highlight powershell %}
Exit-PSSession
{% endhighlight %}

For more information on JEA, I recommend you check out a white paper created by Michael Greene from Microsoft which can be found at the following link - <a title="Michael Greene JEA White Paper" href="http://gallery.technet.microsoft.com/Just-Enough-Administration-6b5ad370" target="_blank">http://gallery.technet.microsoft.com/Just-Enough-Administration-6b5ad370</a>.
