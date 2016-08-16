---
layout: post
title: Controlling LoadBalancer.Org Appliances From PowerShell
date: 2016-08-16 20:00
categories: [devops]
tags: [loadbalancerorg,powershell]
---

First I'd like to say that I'm not being paid or asked to write any of this, but I really do love LoadBalancer.Org appliances. They're a cost effective load balancing solution which combines the best of industry grade open source technologies, together with a WUI (web user interface) for manageability as well as an easy to use CLI (command line interface).

One of the missing features for some people I've talked to has always been the ability to script and automate operations from within PowerShell on Windows. Previously for a Windows Administrator it required having basic knowledge of SSH tunnels, as well as knowing the syntax for the Linux operations.

After seeing this issue, Myself and two colleague's set about creating a useable PowerShell module using the PowerShell approved verbs so that the cmdlets were intuitive to people already using PowerShell.

If you're unsure what the Microsoft approved verbs for commands are, then you can read more about them <a href="https://msdn.microsoft.com/en-us/library/ms714428(v=vs.85).aspx">here</a>.

The outcome of a couple of lunch times hacking together and discussion was the PowerShell module called iControlLBO which we're hosting on GitHub. The repository can be access at <a href="https://github.com/AshleyPoole/iControlLBO">https://github.com/AshleyPoole/iControlLBO<a/>.

While the module is still work in-progress / proof of concept, it can however currently be used to complete the following actions:
- Install module pre-requisites
- Set a real service to draining state
- Set a real service to halted state
- Set a real service to online state
- ALlow the execution of any other CLI commands

As a breif example, if you were to download the module, here's how you would establish a connection to your load balancer and command it to halt a real server (RIP) called 'web1' from the Virtual Service (also known as a VIP) called 'web':
{% highlight powershell %}
Import-Module iControlLBO
Install-LBDependencies

$ssh_con = New-LBConnection -appliance $hostname -username [your-username-here] -password [your-password-here]
Invoke-LBRipHalt -connection $ssh_con -vip web -rip web1
{% endhighlight %}

Feel free to fork the repository and create pull requests for any changes or features you'd like to see! Alternatively, create a issue on the GitHub repository and we can assist in adding features or  support queries.
