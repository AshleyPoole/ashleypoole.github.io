---
layout: post
title: Test Your SSL Servers From Within PowerShell
date: 2015-02-09 08:01
categories: [programming, devops, security]
tags: [powershell, ssl]
---
If you read my blog or follow my <a title="Ashley Poole GitHub" href="https://www.github.com/AshleyPoole/" target="_blank">GitHub</a> account, you're likely to have seen I've recently released a .NET library which is a wrapper for the SSL Labs assessment Api's. These Api's allow the consumer to test their public facing SSL services to detect any configuration issues like certificate chain incomplete, Heartbleed vulnerable, etc.

So why am I mentioning this wrapper again? Well let me explain.

While we were preforming out out of hours maintenance to replace the SSL certificates on our load balancers, I found myself constantly jumping to my browser to run a fresh assessment of our SSL hosts using <a title="SSL Labs Test" href="https://www.ssllabs.com/ssltest/analyze.html" target="_blank">SSL Labs</a>. As you can imagine even with a great testing tool such as SSL Labs the process became laborious and just didn't scale that well (i.e opening multiple scans using multiple browser tabs). I therefore saw this as another great opportunity to use my SSL Labs Api Wrapper (previously called SSLLWrapper) as well as improving our process using automation.
<h2>Welcome ExternalSSLTester.ps1</h2>
ExternalSSLTester.ps1 is written to analyse a host as well as its endpoints and then return this information back to you. The script can also take a list of multiple hosts to ease the life of an IT administrators or DevOps engineer.


{% highlight powershell %}

#######################################################################
# Written by Ashley Poole  -  http://www.ashleypoole.co.uk            #
#                                                                     #
# Checks servers SSL implementation for the given host(s).            #
# This is achieved by consuming SSL Labs Assessment Api Via           #
# SSLLabs Api Wrapper                                                 #
#######################################################################

#region Construction
Param
(
	[Parameter(Mandatory=$false)]
	[ValidateNotNullOrEmpty()]
	[String]
	[alias("host")]
	$hostInput,

	[Parameter(Mandatory=$false)]
	[ValidateNotNullOrEmpty()]
	[String]
	[alias("hosts")]
	$hostsInput,

	[Parameter(Mandatory=$false)]
	[ValidateNotNullOrEmpty()]
	[Bool]
	[alias("endpointdetails")]
	$WriteEndpointDetails
)

# Validating and loading inputs
If (($hostInput) -or ($hostsInput))
{
    If ($hostsInput)
    {
        $Hosts = Get-Content ($PSScriptRoot.ToString() + "\Hosts.txt")
    }
    Else
    {
        $Hosts = @($hostInput)
    }
}
Else
{
    Write-Host "ERROR: No values for host(s) were specified!`r`nSee README files for details on input parameters." -ForegroundColor Red
    Exit
}

# Variables
$SSLLabsApiUrl = "https://api.dev.ssllabs.com/api/fa78d5a4/"

$SSLLWrapperFilePath = $PSScriptRoot.ToString() + "\SSLLWrapper\SSLLWrapper.dll"
$NewtonsoftJsonFilePath = $PSScriptRoot.ToString() + "\SSLLWrapper\NewtonSoft.Json.dll"

# Loading DLLs
Add-Type -Path $NewtonsoftJsonFilePath -ErrorAction Stop
Add-Type -Path $SSLLWrapperFilePath -ErrorAction Stop

# Creating SSLLService
$SSLService = New-Object SSLLWrapper.SSLLService($SSLLabsApiUrl)

#endregion

#region Functions
Function ToBooleanString($value)
{
    return [System.Convert]::ToBoolean($value).ToString()
}

#endregion

clear
Write-Host "`n"
Write-Host "Starting analysis... This process may take several minutes per endpoint, per host..."
Write-Host "`n"

# Testing if Api is online
$ApiInfo = $SSLService.Info()

If ($ApiInfo.Online -ne $true)
{
    # Api is not online. Exiting.
    Write-Host "Api " $SSLLabsApiUrl " is not online, contactable or is incorrect.`r`nExiting." -ForegroundColor Red
    Exit
}

Foreach ($myHost In $Hosts)
{
    # Analysising host with a maximum of a 5 minute wait time
    #$HostAnalysis = $SSLService.AutomaticAnalyze($myHost, 500, 10)
    $HostAnalysis = $SSLService.AutomaticAnalyze($myHost, [SSLLWrapper.SSLLService+Publish]::Off, [SSLLWrapper.SSLLService+ClearCache]::Ignore, [SSLLWrapper.SSLLService+FromCache]::On,[SSLLWrapper.SSLLService+All]::On,500,10)

    Write-Host "**********************************************************************************"
    Write-Host $myHost " - " (Get-Date -format "dd-MM-yyyy HH:mm:ss")
    Write-Host "**********************************************************************************"
    Write-Host "Endpoints #           :" ($HostAnalysis.endpoints).Count
    Write-Host "Analysis Error        :" $HostAnalysis.HasErrorOccurred
    Write-Host "`n"

    Foreach ($Endpoint In $HostAnalysis.endpoints)
    {
        Write-Host "Endpoint              :" $Endpoint.ipAddress

        Write-Host "Grade                 :" $Endpoint.grade
        Write-Host "Has Warnings          :" $Endpoint.hasWarnings

        # Output extra details if EndpointDetails is true and analysis data is good (i.e check grade exists)
        If (($WriteEndpointDetails) -and ($Endpoint.grade))
        {
            $EndpointAnalysis = $SSLService.GetEndpointData($HostAnalysis.host, $Endpoint.ipAddress)

            Write-Host "Server Signature      :" $EndpointAnalysis.Details.serverSignature
            Write-Host "Cert Chain Issue      :" $EndpointAnalysis.Details.chain.issues
            Write-Host "Forward Secrecy       :" $EndpointAnalysis.Details.forwardSecrecy
            Write-Host "Supports RC4          :" $EndpointAnalysis.Details.supportsRc4
            Write-Host "Beast Vulnerable      :" $EndpointAnalysis.Details.vulnBeast
            Write-Host "Heartbleed Vulnerable :" $EndpointAnalysis.Details.heartbleed
            Write-Host "Poodle Vulnerable     :" $EndpointAnalysis.Details.poodleTls

       }
       Write-Host "`n"
    }
    Write-Host "`r`n"
}
{% endhighlight %}

Note - the script currently uses the development SSL Labs Api as it hasn't currently been released for production use yet.
<h2>Usage</h2>
Here is a copy of the <a title="External SSL Tester ReadMe" href="https://github.com/AshleyPoole/PowershellTools/blob/master/Scripts/External_SSL_Tester/README.md" target="_blank">ReadMe</a> file for ExternalSSLTester that describes the usage information.
<blockquote>
<h2>About</h2>
This PowerShell script can be used to check the SSL implementations of any public facing server. To achieve this, the script consumes the SSL Labs Assessment Api using a .NET library called SSL Labs Api Wrapper (previously called SSLLWrapper). This wrapper is also written by myself and can be found at <a title="Github" href="https://github.com/AshleyPoole/SSLLabs-Api-Wrapper" target="_blank">GitHub</a>, <a title="NuGet package" href="https://www.nuget.org/packages/SSLLabsApiWrapper/" target="_blank">NuGet</a> or <a title="Ashley Poole SSL Labs Api Wrapper" href="http://www.ashleypoole.co.uk/ssllabs-api-wrapper" target="_blank">My Website</a>.
<h2><a id="user-content-usage" class="anchor" href="https://github.com/AshleyPoole/PowershellTools/blob/master/Scripts/External_SSL_Tester/README.md#usage"></a>Usage</h2>
ExternalSSLTester.ps1 can be invoked for use with a single host or a predefined selection of hosts which are passed in as a file path. Below are examples of both options:

<strong>Single Host</strong>
<code>ExternalSSLTester.ps1 https://www.ashleypoole.co.uk</code>
<code>ExternalSSLTester.ps1 -host https://www.ashleypoole.co.uk</code>

<strong>Multiple Hosts</strong>
<code>ExternalSSLTester.ps1 -hosts "C:\HostsToCheck.txt"</code>

<strong>Detailed Output</strong>
The script can also be instructed to give a more detailed output for a host's endpoints by using the 'details' parameter. Examples below:
<code>ExternalSSLTester.ps1 https://www.ashleypoole.co.uk -endpointdetails $True</code>
<code>ExternalSSLTester.ps1 -host https://www.ashleypoole.co.uk -endpointdetails $True</code></blockquote>
I've uploaded the script to my <a title="Ashley Poole's PowerShell Tools Repo" href="https://github.com/AshleyPoole/PowershellTools/tree/master/Scripts/External_SSL_Tester" target="_blank">PowerShell tools repository on GitHub</a> where you can also find the required DLL's. Feel free to fork the repository and create pull requests for any improvements to share with the community or comment on this post.
