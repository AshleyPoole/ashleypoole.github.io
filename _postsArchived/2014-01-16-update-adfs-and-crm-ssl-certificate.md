---
layout: post
title: Update ADFS And CRM SSL Certificate
date: 2014-01-16 18:42
categories: [server-admin, devops]
---
<p>1.  Remove (delete) the old cert using MMC on the CRM web servers & ADFS servers.  Verify removal of the cert by reviewing your  IIS https bindings.  We found that if we did not remove the old one first, application of the new one would not work.</p>

<p>2. Add the new cert to the ADFS server first.  Import new cert into MMC cert snapins console. Be sure your 'AppPool user account' has read permissions. You also need to be sure that the 'ADFS service user account' has full permissions to the cert.  Bind new cert to https in IIS.  From your cmd line, perform an IISReset.</p>

<p>3. Add the new cert to your CRM web application servers...all of them if there's more than one.  Import new cert into MMC cert snapins console. Be sure your 'AppPool user account' has read permissions. Bind new cert to https in IIS.  From your cmd line, perform an IISreset.</p>

<p>4.  On your ADFS server, update the cert in ADFS Mgmt Console.  Under Service &gt; certificates &gt; Set service communications certificate to new cert.</p><p>5. Back again to your CRM web servers, fire up the 'Configure Claims Wizard', update to the new certificate, and apply.</p><p>6. On the ADFS server, in the ADFS Mgmt Console, under 'Trust Relationships', update relying trust federation metadata for all instances.</p><p><a href="https://community.dynamics.com/crm/f/117/t/70374.aspx">https://community.dynamics.com/crm/f/117/t/70374.aspx</a></p>
