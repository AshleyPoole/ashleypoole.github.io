---
layout: post
title: Quickly Read IIS Configuration Without Using Feature Delegation
date: 2014-07-28 07:03
categories: [devops, server-admin]
tags: [iis]
---
So you're a developer or someone with a read only user account on your production web servers. Your IT administrators had deemed that <a title="IIS Feature Delegation" href="http://technet.microsoft.com/en-us/library/cc770505(v=ws.10).aspx" target="_blank">feature delegation in IIS</a> isn't needed so you're unable to view the IIS configuration. You've been managing the production support just fine till now and the pay cheques kept rolling in. Then one day your handed a production outage of a business critical web application that the business needs resolving quickly but you believe it's IIS related.

You can go and view the web.config for the web application but you need to see the actual configuration for the website / application from an IIS point of view. What do you do, hunt down an IT administrator, Update your CV or pray that a server reboot helps?

One way around this is to view the IIS configuration directly!

Viewing %windir%\system32\inet\srv\config\applicationHost.config will allow you to see the configuration of the websites and applications within IIS, even as a read only user.
