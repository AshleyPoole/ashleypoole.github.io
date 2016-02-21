---
layout: post
title: Another Tesco (Bank) Security Fail!
ate: 2016-02-18 20:50
categories: [security]
tags: [wtf, rant]
---

Tesco doesn't have a great track record with security as Troy Hunt has pointed out in the past with such articles as <a href="http://www.troyhunt.com/2014/02/the-tesco-hack-heres-how-it-probably.html" target="_blank">The Tesco hack – here’s how it (probably) happened</a> and <a href="http://www.troyhunt.com/2012/07/lessons-in-website-security-anti.html" target="_blank">Lessons in website security anti-patterns by Tesco</a>. I was still surprised though when I attempted to log into my online account at  Tesco Bank with only my username (that turned out to be incorrect), only to be greeted with a page informing me that my username didn't exist. Hang on a minute, what just happened there?!

Here's a nice animated GIF of Tesco Bank disclosing the presence of usernames for their **BANKING** customers. Yes, you read that right, I said banking customers! Of all the systems you'd want to make sure you don't disclose the presence of user accounts, you'd hope your banks online login system wasn't one of them!

<img src="{{ "/img/2016/02/tesco-bank-login.gif" | prepend: site.assetsbaseurl }}" alt="tesco bank login" width="694" height="499" style="margin:0px auto;display:block;border:3px solid black"/>

During the recording of the GIF on my Windows machine, I also noticed Internet Explorer asked me if I wanted AutoComplete to remember my username. I was surprised by this and immediately took to viewing the source of the login page in order to dig into it a little more.

<img src="{{ "/img/2016/02/tesco-bank-autocomplete.png" | prepend: site.assetsbaseurl }}" alt="tesco bank autocomplete" width="693" height="493" style="margin:0px auto;display:block;border:3px solid black"/>

As you can see from the above screenshot of the source of Tesco Bank's login page, the username input field (login-uid) doesn't have the autocomplete="off" attribute set, so the browsers autocomplete feature offered to store my username. It get's even worse though on the first page of their "forgotten your username" process that asks for your credit card number along with other personal information such as name and DoB, yet none of these fields have AutoComplete disabled either!

<img src="{{ "/img/2016/02/tesco-bank-autocomplete-card-number.png" | prepend: site.assetsbaseurl }}" alt="tesco bank autocomplete credit card number" width="711" height="516" style="margin:0px auto;display:block;border:3px solid black"/>

Like seriously Tesco, have you not heard of public computers and how letting the browser offer to save your username yet alone a credit card number could be dangerous?! I'd like to have thought Tesco would have learned their lesson the <a href="http://www.theguardian.com/technology/2014/feb/14/tesco-customer-accounts-suspended-hacker-attack">first time around</a>!

To round this <del>rant</del> post off, I decided to <a href="https://twitter.com/AshleyPooleUK/status/700022730448596996" target="_blank">tweet</a> Tesco Bank about my concern with their user disclosure though to no avail as you can see below. Thanks for reading and here's to hoping you don't bank with Tesco too! Ha.

<img src="{{ "/img/2016/02/tesco-bank-twitter.gif" | prepend: site.assetsbaseurl }}" alt="tesco bank twitter conversation" width="416" height="890" style="margin:0px auto;display:block;border:3px solid black"/>
