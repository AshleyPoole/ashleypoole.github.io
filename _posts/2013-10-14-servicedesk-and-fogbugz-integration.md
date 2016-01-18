---
layout: post
title: ServiceDesk And FogBugz Integration Notes (Prototype)
date: 2013-10-14 20:15
categories: [general, programming]
---
Notes from my implementation of integration between ServiceDesk and FogBugz with a C# application.

#### General   
Using Enviroment.CommandLine turned out to an improvement over args[] as the way command line arguments are normally parsed. Basically the double quotes were being removed when parsed which meant the Json being input into the program as an command line argument was no longer valid using args[].

#### ServiceDesk  
Using the External Action Plugin it is possible to write a plugin external using Java. ManageEnginee have also released a Java plugin that is designed to called an external script or program which is passed a Json containing the case details. I have gone down the second path as it allows coding to take place in .NET environment ( C# ). Returning a valid Json back to ServiceDesk will carry out certain instruction listed like updating the Notes field or setting a custom field.

#### FogBugz  
I initially used a API wrapper for FogBugz called FogLampz. This had limitations which consisted of throwing exceptions on custom fields used in FogBugz and strangely would never return the created case ID (returned null's which is most likely to the exceptions being thrown). The documentation was also very poor which resulted in the need to fork the repo which lead to some answers like discovering the exceptions being thrown though not all of them. Due to this fact, I stopped using FogLampz and decided to talk to the API myself creating an API class to handle the direct interaction.

FogBugz XML API simply takes arguments passed as part of the request and returns an XML response with an root element of &lt;response&gt;. If the first child node is &lt;error&gt; then something went wrong.

The API is religiously UTF-8. All requests must contain a token argument.

#### ServiceDesk Resources  
External Action Plugin Documentation - http://www.manageengine.com/products/service-desk/help/adminguide/api/external-action-plugin.html
External Action Plugin Documentation (Form Thread) - https://forums.manageengine.com/topic/external-action-plugin-feature-in-sdp
ServiceDesk API Documentation - http://www.manageengine.com/products/service-desk/help/adminguide/api/request-operations.html

#### FogBugz Resources  
FogBugz API Version 7 - http://www.fogcreek.com/fogbugz/docs/70/topics/advanced/api.html
FogBugz API Version 8 - http://help.fogcreek.com/8202/xml-api
BugScout (Native replacement in main API version 8) - http://www.fogcreek.com/fogbugz/docs/70/topics/customers/BugzScout.html
URL Trigger Plugin - http://www.fogcreek.com/fogbugz/docs/70/topics/plugins/URLTrigger.html
FogLampz API Wrapper - https://foglampz.codeplex.com/


#### General Resources  
.NET Json Framework - http://james.newtonking.com/json

#### Installation  
Copy ScriptActionConfig.xml and Request_ActionMenu.xml to [Install Location]ManageEngineServiceDeskserverdefaultconf  

Copy ScriptExecution.jar to [Install Location]ManageEngineServiceDeskserverdefaultlib
