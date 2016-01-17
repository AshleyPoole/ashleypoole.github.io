---
layout: post
title: LoadBalancer.org Heartbeat Notifications
date: 2013-08-17 18:10
categories: [devops, server-admin]
tags: [load-balancer-org]
---
Below explains how to setup email notifications for Loadbalancer.org

### Heartbeat notification - Step by Step
SSH onto the master as root
Type vi /etc/ha.d/haresources
Append the following to the end of the line starting with lbmaster - MailTo::ashley@ashleypoole.co.uk
Press excape key, then type :wq
On the console type yum install mailx -y
In the Admin portal goto Edit Configuration > Physical - Advanced Configuration
Restart hearbeat either via the Admin Portal or issuing the following command in the terminal service heartbeat reload
Complete the same on the slave

Currently it doesn't support notifications for on a layer 7 VIP when a real server has gone offline though it is visible in the Admin portal. They say this is a up coming feature for layer 7 VIP's.
They recommend a custom script for parsing log files which should be easy enough to implement should we want to.
