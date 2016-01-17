---
layout: post
title: xWebVirtualDirectory Mandatory WebApplication Parameter Blocks VD Creation
date: 2014-09-06 21:11
categories: [devops, knowledge-base]
tags: [powershell, dsc]
---
Searching through the vast online realm that we call the internet, your find a new stream of PowerShell Desired State Configuration (DSC) resources being created daily to configure anything from Active Directory to the Chrome browser. Microsoft the creator of PowerShell DSC publish <a title="MS DSC Resource Kit" href="http://gallery.technet.microsoft.com/scriptcenter/DSC-Resource-Kit-All-c449312d" target="_blank">their own DSC resource kit</a>, as well as encouraging the community to create their own resources.

Working a lot with Microsofts xWebVirtualDirectory resource (part of the <a title="xWebAdministration Module" href="http://gallery.technet.microsoft.com/scriptcenter/xWebAdministration-Module-3c8bb6be" target="_blank">xWebAdministrator module</a>) recently lead me to stumble upon what I believe is a design flaw in the resource. This design flaw was that the WebApplication parameter was mandatory though didn't allow empty or null strings which meant I was unable to create virtual directories that weren't sat under a web application. Running the cmdlet New-WebVirtualDirectory directly also confirmed to myself that the WebApplication parameter wasn't required though did allow empty strings to be used if declared.

After spending sometime unsuccessfully trying to work out a way to pass a null or empty string as the WebApplication parameter I decided to explorer the code behind the xWebVirtualDirectory resource where I discovered this was because the mandatory attribute was used without allowing empty strings. I have therefore had to modify this behaviour for my usage of the resource so wanted to share my modified version.

Below you can find a link to the modified version of the resources psm1 file on my Github, alternatively, you can search through your own copy of the resources psm1 file and add the [AllowEmtpyString()] attitude to every $WebApplication or $Application parameter.

<a title="Ashley Poole's Github xWebVirtualDirectory" href="https://github.com/AshleyPoole/PowerShell-DSC-Resources/blob/d1d2ad670cdc437867da2389cbadf2a886427b9b/xWebAdministration/DSCResources/MSFT_xWebVirtualDirectory/MSFT_xWebVirtualDirectory.psm1" target="_blank">https://github.com/AshleyPoole/PowerShell-DSC-Resources/blob/d1d2ad670cdc437867da2389cbadf2a886427b9b/xWebAdministration/DSCResources/MSFT_xWebVirtualDirectory/MSFT_xWebVirtualDirectory.psm1 </a>
