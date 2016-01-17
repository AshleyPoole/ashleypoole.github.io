---
layout: post
title: Manipulating Exchange Mailboxs Using Powershell
date: 2013-08-19 17:59
categories: [server-admin]
tags: [powershell, exchange]
---
In the Exchange 2010 mailbox creation wizard, there is no longer an option to create a shared mailbox. Below is the command to create a shared mailbox using Powershell.
{% highlight powershell %}
New-Mailbox –Name "Customer Service" –Alias customerservice –UserPrincipalName customerservice@ashleypoole.co.uk -Shared
{% endhighlight %}





Add / Remove mailbox permissions
{% highlight powershell %}
Add-MailboxPermission -Identity user.one@ashleypoole.co.uk -User user.two@protolabs.co.uk -AccessRights FullAccess
Remove-MailboxPermission -Identity user.one@ashleypoole.co.uk -User user.two@protolabs.co.uk -AccessRights FullAccess
{% endhighlight %}
