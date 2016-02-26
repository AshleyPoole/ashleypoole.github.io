---
layout: post
title: What Is DevOps?
date: 2016-02-26 07:00
categories: [devops]
tags: [questions, decodingdevopsseries]
---

DevOps is an interesting subject and one that's close to my heart, so I've decided to write a short series over the next few months on DevOps called **Decoding DevOps**. This series also includes my interpretations of the current state of the DevOps movement, so without delay, let's crack on with the first post in the series.

## What Is DevOps?
I mean, what truly is DevOps? I'm confident in saying I've done DevOps, I've "been" DevOps and I'd also personally say I'm still doing DevOps but what actually is DevOps? Confused, well I think that's the current state of DevOps; I see it as another new (or not so new) hype word that's thrown around who's definition widely varies and in some cases, even abused.

*Maybe abused was a strong choice of words as DevOps does mean different things to different people and that is of course well within their right. After all, DevOps is all about solving problems right?!*

Let's get back to the question at hand by having a look at Wikipedia's definition of DevOps:

> DevOps is a culture, movement or practice that emphasizes the collaboration and communication of both software developers and other information-technology (IT) professionals while automating the process of software delivery and infrastructure changes. It aims at establishing a culture and environment where building, testing, and releasing software, can happen rapidly, frequently, and more reliably.
> https://en.wikipedia.org/wiki/DevOps

Never one to take Wikipedia as the source of truth, I decided to look online for another reference and was surprised to find that the UK Government publishes their own definition of DevOps which is:  

> DevOps is a cultural and professional movement in response to the mistakes commonly made by large organisations. Often organisations will have very separate units for development, quality assurance and operations business. In extreme cases these units may be based in different locations, work for different organisations and under completely different management structures. This is what DevOps aims to correct. It is not a methodology or framework, but a set of principles and a willingness to break down silos.  
> https://www.gov.uk/service-manual/operations/devops.html

As you can see the definitions are very similar and hopefully you now have a basic understanding of what DevOps is about. I want to emphasise the point that gov.uk makes which is that DevOps should be a willingness to break down silos but not to become another silo in itself - this statement directly relates to the core principles of DevOps which we'll explore next.

## Core Principles Of DevOps
In my opinion based on my current understanding and the referenced definitions, DevOps can can be summarised into the following list of high level core principles:  
- **Communication**  
- **Collaboration**  
- **Integration**  

<img src="{{ "/img/2016/02/dev-ops-principles.jpg" | prepend: site.assetsbaseurl }}" alt="devops principles" width="794" height="354" style="margin:0px auto;display:block"/>
<center><i>Image credit: NewRelic.</i></center>  

A common example that the DevOps core principles are trying to solve is the scenario where Development (Dev) would historically "<a href="https://en.wikipedia.org/wiki/Traditional_engineering">throw code over the wall</a>" at the IT Operations (Ops) team and let them deal with the fall out of any production issues including stability of the system. With DevOps, both Operations and Development should work together to ensure both teams objectives are met - Delivering change faster but in a stable manner in order to support the business's objectives.

<img src="{{ "/img/2016/02/worked-fine-in-dev.jpg" | prepend: site.assetsbaseurl }}" alt="Worked Fine In Dev, Ops Problem Now" width="400" height="299" style="margin:0px auto;display:block"/>
<center><i>Historical problem of Dev throwing code over the wall to production / Ops.</i></center>  

This previous lack of communication, collaboration and integration would then to lead to Ops pushing back on future releases and wanting to slow down the release process in order to maintain the stability and available of the system.

<img src="{{ "/img/2016/02/ops-vs-dev-support.png" | prepend: site.assetsbaseurl }}" alt="Ops pushing back at dev" width="415" height="265" style="margin:0px auto;display:block"/>
<center><i>Historical problem of Ops reacting to Dev throwing code over the wall and impacting the stability and available of production.</i></center>

## Where Does DevOps Fit In Within The Business Process?

I see DevOps as an extension of the Agile business process that shares a number of common objectives which ultimately are to support change for the business but from different levels of the stack. In this case where Agile is about bringing the business and Development teams together in order to support change, DevOps is about archiving similar objectives by bringing Development and Operations teams together.

<img src="{{ "/img/2016/02/agile-devops-fit.jpg" | prepend: site.assetsbaseurl }}" alt="agile and devops fit" width="669" height="397" style="margin:0px auto;display:block"/>
<center><i>Image credit: Stackify - <a href="http://stackify.com/defining-the-ops-in-devops/">Define the Ops in Devops</a> article.</i></center>

## Summary
In an attempt to keep this first post of the series reasonable short, I will talk more about DevOps culture, responsibilities, automation and tooling in future posts. The next post will focus on whether DevOps is a role, team or culture.

To summarise this post, DevOps is about:  
- Bridging the gap between operations and development by improving communication, collaboration and integration, similar to that of Agile which bridges the gap between the business and development  
- Breaking down silo's, not creating another silo  
- Enabling IT agility and operational excellence in order to support change requested by businesses objectives  
- Helping release more often without compromising the stability and available of the systems  
