---
layout: post
title: Bind Partial Payloads To Data Models Using WebAPI
date: 2014-08-16 20:05
categories: [programming, knowledge-base]
---
A while back I was working on a MVC 4 web application that used WebAPI's where I wanted my received payload directly bound to my data model which worked great. An issue however arose when a partial payload was received as the binding to my data model would thrown an exception due to being incomplete vs my data models definition.

After researching the topic and knowing that the Newtonsoft.Json library was used, I discovered you could override the null value handling within the library which was previously causing the exception to be thrown on partial payloads. Below is an example WebApiConfig.cs that demonstrates how to override the default null value handing of the json serialiser. The particular line you need is #11.

If using this solution to ensure partial payloads are bound to your data model, you will most likely have to ensure you have application logic to also handle null or empty values when accessing your data model.

{% highlight c# linenos %}
using System.Web.Http;
using Newtonsoft.Json;

namespace ForefrontSolutions.Integrator.Web
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
			// Allow partial payloads when serializing
			config.Formatters.JsonFormatter.SerializerSettings = new JsonSerializerSettings { NullValueHandling = NullValueHandling.Ignore };

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: &quot;DefaultApi&quot;,
                routeTemplate: &quot;api/{controller}/{id}&quot;,
                defaults: new { id = RouteParameter.Optional }
            );
        }
    }
}

{% endhighlight %}
