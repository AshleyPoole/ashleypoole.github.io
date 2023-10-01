---
layout: post
title: Push ManageEngine ServiceDesk Cases Into FogCreek FogBugz (Prototype)
date: 2014-10-14 20:03
categories: [programming, devops]
excerpt_separator: <!--more-->
---
<p class="p1">Recently I volunteered to take on a project that required developing integration between ManageEngine's ServiceDesk and FogCreek's FogBugz installations - merely separated by 3800 miles!</p>

<!--more-->

<p class="p1">As part of this project I developed a prototype in my own time based on the business requirements at that time which were to provide the ability to 'push' cases from ServiceDesk into FogBugz. Another business requirement for the integration was to also reference the FogBugz case ID in the ServiceDesk request and vice versa for tracking purposes.</p>

<p class="p1">As the prototype was completed and demo'd, we quickly realised we required deeper integration - not just the one way push ability that we initial envisioned would fulfill our needs. Therefore as you can imagine the production version when finished also contained many extra features like one that allowed FogBugz case updates to automatically follow back into the ServiceDesk and update the ServiceDesk original request. This feature was based on the user story that "users must only have one interface to check the status of their cases".</p>
<img class="aligncenter size-full wp-image-1291" src="{{ "/img/2014/06/servicedesk-fogbugz.png" | prepend: site.assetsbaseurl }}" alt="servicedesk fogbugz" width="348" height="172" />


I produced the prototype as a C# console application which both consumes and outputs <a title="JSON W3Schools" href="http://www.w3schools.com/json/" target="_blank">JSON</a>. The input JSON is passed in when executed by ServiceDesk's external action plugin interface. The prototype then parses the JSON which contains the users request, then passes this over the wire to the FogBugz XML API in an accepted format. The response from the FogBugz API is then used to create a JSON object which is written back to standard output for the ServiceDesk external action plugin interface to parse and action.

<p class="p1">Below is a link to the prototype which I've uploaded to Github where you can find the source code should you wish to have a look and hopefully be of help to someone out there trying to accomplish the same type of integration between these products! Now for the warning - Being as this is a prototype, it <b>is</b> a little rough around the edges and best coding practices have likely not been followed. This was simply a prototype to show integration of some kind could be achieved before committing time to the main integration project.</p>
<a title="GitHub Project Link" href="https://github.com/AshleyPoole/TicketingIntegrator-Prototype/" target="_blank">https://github.com/AshleyPoole/TicketingIntegrator-Prototype/</a>

<strong>Integration 1.0 - Production</strong>

I ended up writing the production version of the integration project as a ASP.NET MVC website using WebApi's which sit between the two systems to facilitate communication and integration. I also correctly used data models which I drastically helped with data binding and translation between the two products.

When binding the payload directly to a data model that was issued to the WebApi's I did have to overcome one issue with partial JSON binding. Check out my "<a title="bind partial payloads to data models using webapi" href="{% post_url 2014-08-16-bind-partial-payloads-to-data-models-using-webapi %}" target="_blank">Bind Partial Payloads To Data Models Using WebAPI</a>" post for more details on that.

<span style="font-size: 15.5555562973022px;">I also created a custom C# client (similar to the prototype) that plugged into ServiceDesk's external action interface which also consumes the WebApi's. On the FogBugz side of things, the FogBugz Url Triggers plugin was used to setup rules that fired data at the WebApi's when activity on any cases occurred.</span>

Sadly I can't share the Integration 1.0 code but good luck on your own integration project!
