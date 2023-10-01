---
layout: post
title: Loadbalancer.org Solution Notes
date: 2013-08-17 19:09
categories: [knowledge-base, server-admin]
tags: [load-balancer-org]
published: false
---
List of notes relevant to the Loadbalancer.org solution

Operating system - runs on a custom version of CentOS which is based on Red Hat Enterprise Linux.
Proxy component - uses the HAPROXY which is a linux based package (http://haproxy.1wt.eu/).
SSL Termination - uses the POUND which is a linux based package (Apsis Gmbh )
Shell - Yes a full bash shell is available via SSH (port 22 by default).
Shell Access - authentication can either be done via password or SSH keys. This will also affect replication between the nodes. See below point.
Node Replication - Replicates configs using SCP.
Failover - uses linux package called Heartbeat to handle failover. Basicly monitors each server and allocates shared resources like HAPROXY (Linux-HA )
Ports - Ports that the solution uses so can't be used - 22 SSH, 7777 HAPROXY, 9080 WUI HTTP, 9443 WUI HTTPS and 9081 NGINX Server Fallback page


Setup Notes
Create the slave node first
Under Layer 7 Advanced - Persistance Table Replication = True
For TProxy - The servers gateway must be set to the virtual IP of the load balancing group
Certs - Must be in base 64 encoded
After setup is complete in /etc/haproxy/loadbalancer.org change "option httpclose" to "#option httpclose". It can also be changed to "option http-server-close" though we know that the first solution will be included in the WUI in a upcomming release
Under layer 4 advanced options set AUTO NAT to eth0


Other Notes
Layer 7 virtual servers includes persistence modes including HTTP cookie and Source IP
Layer 7 virtual servers are configured different to Layer 4 because it uses Linux Virtual Server where Layer 7 uses HAPROXY
When HTTP cookie persistence mode is used then the cookie name is set to be the real server label / name
Any chances to HAPROXY requires a reload of the service
When TPROXY is enabled within the Layer 7 virtual server x-forwarded-for isn't required as the clients IP is preserved


Initial Pricing - Online Pricing From Website (July 2013) - Though quotes from Sales can include discounts
2 * Enterprise VA Clustered HA with 3 years standard support (9am-5pm) = £5193.50
2 * Enterprise VA Clustered HA with 3 years premium support (24x7) = £5992.50
2 * Enterprise VA Clustered HA with 90 days installation support = £3995.00


Q & A
Q: What happens if all real servers fail when in a layer 7 setup?
A: The load balancer will continue trying to route requests through to the real servers. Only when all real servers are disabled via the config or GUI will the load balancer display a generic this site is down page. (For a layer 4 setup, the down page will be displayed automatically). -- Correction if they fail the PING health check they will be removed.

Q: What serves the down page?
A: The load balancer uses NGINX to host and serve up the down page

Q: Can the generic down page be modified?
A: Yes, this can be done via the GUI (Maintenance > Fallback Page) or /usr/share/nginx/html/

Q: Where are the heartbeat log entries written to?
A : They are written to /var/log/messages as well as it's only log at /var/log/ha.log

Q: Can email alerts be setup for when the master/slave failover the active role?
A: Yes, this can be manually added to the heartbeat configuration, see my post here on more details - Loadbalancer.org Email Notifications Configurations

Q: What are release / development cycle time frames?
A: They release between 3-4 major feature updates each year with several service updates throughout the year

Q: Are updates covered under the support package
A: Yes they are free if you have a support package
