---
layout: post
title: Converting Certificates Using OpenSSL
date: 2014-01-18 19:16
categories: [devops, server-admin]
tags: [ssl]
---
Below is a brief example of how to use OpenSSL on Windows to convert certificates from one format to another. I have personally found this useful for converting PFX certificates to PEM format for use on LoadBalancer.org devices.

If you don't have OpenSSL for Windows installed, head over to <a href="http://www.openssl.org/related/binaries.html" target="_blank">http://www.openssl.org/related/binaries.html</a> where you will find a link to <a href="http://slproweb.com/products/Win32OpenSSL.html" target="_blank">http://slproweb.com/products/Win32OpenSSL.html</a> for the installer.

Converting a PFX to PEM format example. Note you will be prompted to provide the certificate password.

{% highlight powershell %}
openssl pkcs12 -in c:t\empcert.pfx -nodes -out c:\tempcert.pem

openssl x509 -in c:\tempcert.cer -inform DER -out c:\tempcert.pem -outform PEM
{% endhighlight %}
