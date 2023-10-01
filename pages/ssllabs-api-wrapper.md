---
layout: page
title: Qualys SSL Labs Api Wrapper
sitemap:
  changefreq: quarterly
permalink: /ssllabs-api-wrapper/
---
** This package is no longer maintained **

## What Is this wrapper?
This is the first publicly available .NET wrapper developed for the <a title="SSL Labs Assessment API's GitHub" href="https://github.com/ssllabs/ssllabs-scan/blob/master/ssllabs-api-docs.md" target="_blank">SSL Labs' Assessment API's</a> that allow the consumer to test SSL servers on the public internet.

This wrapper easies the communication to the API's for .NET developers which allows you as the developer to focus on your project rather than managing the plumbing and overhead required to consume the API's.

<strong>Notes</strong>
- The wrapper does <strong>NOT</strong> use web scrapping like other wrappers which don't use the assessment API's.  

<h2>NuGet Package</h2>
The wrapper can easily be imported into your project using the <a title="SSL Labs Api Wrapper NuGet Package" href="https://www.nuget.org/packages/SSLLabsApiWrapper/" target="_blank">NuGet package</a>. The NuGet install command for this package is:

{% highlight powershell %}
PM> Install-Package SSLLabsApiWrapper
{% endhighlight %}

<h2>Wrapper Usage</h2>
When creating a new instance of the SSL Labs Api wrapper service you must supply the API url during the initialisation. For example in C# this would be expressed as the following:

{% highlight c# %}
var ssllService = new SSLLabsApiWrapper.SSLLabsApiService("https://api.ssllabs.com/api/v2");

// Or if you use the SSLWrapper namespace this can be shorten to  
var ssllService = new SSLLabsApiService("https://api.ssllabs.com/api/v2");
{% endhighlight %}

<h3>Methods</h3>
Below are the method signatures of the SSL Labs Api wrapper service.

<h4>Info()</h4>
The Info method is used to determine if the API is online and returns an Info response object. No input parameters are taken.

{% highlight c# %}
public Info Info()
{% endhighlight %}

<h4>Analyze()</h4>
The Analyze method is used to initiate an assessment or retrieve results and returns an Analyze response object. The results may only be partial so see the SSL Labs documentation for more information as as GetEndpointDetails call may be needed to view the whole result set.

{% highlight c# %}
public Analyze Analyze(string host, Publish publish, StartNew startNew, FromCache fromCache, ?int maxHours, IgnoreMismatch ignoreMismatch, All all)
{% endhighlight %}

The wrapper also contains an overloaded Analyze method which only requires the host parameter. Internal is uses the following parameter options - Publish.Off, StartNew.On, FromCache.Ignore, All.On.

{% highlight c# %}
public Analyze Analyze(string host)
{% endhighlight %}

<h4>AutomaticAnalyze()</h4>
The Analyze method is used to initiate and wait for an assessment to complete before retrieving results. Compared to the normal Analyze() method this method keeps checking the Api and only when a scan has finished does it return. This saves the consumer from having to write their own logic for handling an assessment in progress. Another call to GetEndpointDetails() may be needed to view the whole result set for a given endpoint.

{% highlight c# %}
public Analyze AutomaticAnalyze(string host, Publish publish, StartNew startNew, FromCache fromCache, ?int maxHours, IgnoreMismatch ignoreMismatch, All all)
{% endhighlight %}

The wrapper also contains an overloaded AutomaticAnalyze method which only requires the host parameter. Internal is uses the following parameter options - Publish.Off, StartNew.On, FromCache.Ignore, All.On.

{% highlight c# %}
public Analyze AutomaticAnalyze(string host)
{% endhighlight %}

<h4>GetEndpointData()</h4>
The GetEndpointData method is used to retrieve a full results set for a endpoint and returns an Endpoint response endpoint.

{% highlight c# %}
public Endpoint GetEndpointData(string host, string s, FromCache fromCache)
{% endhighlight %}

The wrapper also contains an overloaded GetEndpointData method which only requires the host and s parameter. Internal it uses FromCache.Off.

{% highlight c# %}
public Endpoint GetEndpointData(string host, string s)
{% endhighlight %}

<h4>GetStatusCodes()</h4>
The GetStatusCodes method is use to retrieve a list of status codes and messages which is returned as a StatusCodes response object.

{% highlight c# %}
public StatusDetails GetStatusCodes()
{% endhighlight %}

<h3>Response Objects</h3>
All the response objects are static .NET objects which are populated from the SSL Labs Assessment API's result. Due to the response models are based on those derived from the API itself I will only provide a top level response model map. For more information on the properties available check out <a title="SSL Labs API Responses" href="(https://github.com/ssllabs/ssllabs-scan/blob/master/ssllabs-api-docs.md#response-objects" target="_blank">their documentation</a>.

All response objects also inherit from a custom BaseModel to extent the usability to you as the developer, as well as core functionality needed to consume the API's. The properties are listed below for all top level or custom response objects.

A property may be NULL or 0 if the field was NULL or not listed in the API's response.
<h4>BaseModel</h4>

{% highlight c# %}
public Header Header { get; set; }
public bool HasErrorOccurred { get; set; }
public List<Error> Errors { get; set; }
public Wrapper Wrapper { get; set; }

public class Header
{
  public int statusCode { get; set; }
  public string statusDescription { get; set; }
}

public class Error
{
	public string field { get; set; }
	public string message { get; set; }
}

public class Wrapper
{
    public int ApiPassCount { get; set; }
    public string ApiCommandUrl { get; set; }
    public string ApiRawResponse { get; set; }
}
{% endhighlight %}

<h4>Info</h4>

{% highlight c# %}
public string engineVersion { get; set; }
public string criteriaVersion { get; set; }
public int clientMaxAssessments { get; set; }
public int currentAssessments { get; set; }
public List<string> messages { get; set; }
public bool Online { get; set; }
{% endhighlight %}

<h4>Analyze</h4>

{% highlight c# %}
public string host { get; set; }
public int port { get; set; }
public string protocol { get; set; }
public bool isPublic { get; set; }
public string status { get; set; }
public long startTime { get; set; }
public string engineVersion { get; set; }
public string criteriaVersion { get; set; }
public List<Endpoint> endpoints { get; set; }
{% endhighlight %}

<h4>Endpoint</h4>

{% highlight c# %}
public string ipAddress { get; set; }
public string statusMessage { get; set; }
public string statusDetails { get; set; }
public string statusDetailsMessage { get; set; }
public int progress { get; set; }
public int eta { get; set; }
public int delegation { get; set; }
public int duration { get; set; }
public string grade { get; set; }
public bool hasWarnings { get; set; }
public bool isExceptional { get; set; }
public Details Details { get; set; }
{% endhighlight %}

<h4>StatusDetails</h4>

{% highlight c# %}
public StatusDetails StatusDetails { get; set; }
{% endhighlight %}

<h3>Custom Parameters</h3>
Custom parameters are as follows:

{% highlight c# %}
public enum Publish
{
    On,
    Off
}

public enum StartNew
{
    On,
	Ignore
}

public enum FromCache
{
    On,
    Off,
	Ignore
}

public enum All
{
    On,
    Done
}

public enum IgnoreMismatch
{
    On,
    Off
}
{% endhighlight %}

<h2>Contributing</h2>
If you wish to contribute or view the source code for the wrapper, you can do so from the <a title="SSL Labs Api Wrapper GitHub Page" href="https://github.com/AshleyPoole/ssllabs-api-wrapper" target="_blank">project's GitHub page</a>.
