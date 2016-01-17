---
layout: post
title: Windows Pending Reboot Flag
date: 2013-08-19 18:02
categories: [server-admin, knowledge-base]
tags: [windows]
---
To clear the pending reboot flag on Windows, complete the below registry key change:

1. Regedit
2. Navigate to "HKEY_LOCAL_MACHINESYSTEMCurrentControlSetControlSession Manager"
3. Rename the "PendingFileRenameOperations" value to something like "PendingFileRenameOperations2"

Done!
