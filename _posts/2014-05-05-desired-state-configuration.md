---
layout: post
title: Quick Desired State Configuration Example
date: 2014-05-05 09:45
categories: [Blog, DevOps, DSC, Knowledge Base, Powershell, Powershell, Server Administration]
---
Desired State Configuration (DSC) allows for the ability to configure the state of Windows devices running Powershell V4. This is very similar to solutions like Chef and Puppet which mainly focus on Linux based devices although DSC can be tied into these other solutions to configure Windows devices. The stages to DSC are:
<ol>
	<li>Author - Creating your configuration</li>
	<li>Stage - Converting your configuration to .MOF</li>
	<li>Apply - Running the .MOF file (Your configuration) .MOF stands for Management Object Format.</li>
</ol>
More on the .MOF can be found <a style="font-family: sans-serif; font-size: medium; font-style: normal; font-variant: normal; line-height: normal;" title="Managed Object Format" href="http://msdn.microsoft.com/en-us/library/aa823192(v=vs.85).aspx" target="_blank">here</a>.
<h2>Basic DSC Example</h2>
<h4>Author</h4>

{% highlight powershell %}
Configuration WebServer # Configuration name 
{
    param ( $NodeName ) # Server name as parameter

    Node $NodeName # Configuration block for each server
    {
        # Install IIS       
        WindowsFeature IIS
        {            
            Ensure = "Present" # 'Absent' or 'Present'
            Name = "Web-Server"         
        }

        # Install MyWebsite Folder
        File MyWebsiteDirectory
        {
            Ensure = "Present"
            Type = "Directory" # 'File' or 'Directory'
            SourcePath = $MyWebsiteFiles # Source files to populate destination
            DestinationPath = "E:websMyWebsite"
            DependsOn = "WindowsFeature]IIS # Depend on 'WindowsFeature IIS'
        }
    }
 }
{% endhighlight %}

<h4>Staging</h4>
The below command will compile the DSC configuration called WebServer to .MOF files for each NodeName listed.

{% highlight powershell %}
WebServer -NodeName "SVR01","SVR02"
{% endhighlight %}

<h4>Apply</h4>
To apply the configuration, your need to use the Start-DSCConfiguration cmdlet which is part of the PSDesiredStateConfiguration module. The cmdlet will copy the .MOF files to the specified servers and use the specified servers Local Configuration Manager (LCM) engine to comply with the configuration.

{% highlight powershell %}
Start-DSCConfiguration -Path .WebServer -Wait -Verbose -Force
{% endhighlight %}


For a list of other built in DSC resources check <a title="DSC Built In Resources" href="http://technet.microsoft.com/en-us/library/dn249921.aspx" target="_blank">here</a>.
