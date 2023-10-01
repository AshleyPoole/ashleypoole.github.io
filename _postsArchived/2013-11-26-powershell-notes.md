---
layout: post
title: Powershell Notes
date: 2013-11-26 14:28
categories: [knowledge-base, devops, programming]
tags: [powershell]
---
This is an ongoing collection of notes related to my learning of Powershell. Warning - These notes aren't structured yet.

Powershell unlike other CLI's is object based not string based, even though the output may appear to be a string sometimes.

Powershell has something called PS drives which are providers to allow interaction to components whether it be Filesystems, Registry, Variables, etc though see them as you would a filesystem. These are only Powershell drives and drive names must follow with a colon.*

Show what PS drives are available
{% highlight powershell %}
get-psdrive
{% endhighlight %}

Either will show system variables. This is using a PSDrive. The end colon is important.
{% highlight powershell %}
ls variable:
dir variable:
{% endhighlight %}

This would change your current 'drive' to HKey Current User registry.
{% highlight powershell %}
cd hkcu
{% endhighlight %}

Creates a new PSDrive called Demo as a filesystem provider going to C:demo
{% highlight powershell %}
New-PSDrive -name Demo -PSProvider FileSystem -Root C:Demo
{% endhighlight %}

If you run items like Net Use for example, behind the scenes Powershell will run CMD.exe, run the command and return the output to Powershell.

cmdlets are always a Verbal followed a dash then a singular noun.

This will search powershell help for any help topics that names matches the work. The 'man' command also works like Linux. (I prefer 'man' command).
{% highlight powershell %}
help *word*

i.e
help *mail*
{% endhighlight %}

If in the help, the parameter name is in squared brackets, this means the parameter name is optional and therefore based on the order. parameter names can be shortened though need to take into account common parameter too as may need to provide more detail (length of shortened name).

For string[] arrays, you can just use comma separated lists. It's important to wrap each parameter in single quotes only if contains a space, not the whole string array.

Get all commands containing *word*. Great way to discover new commands. Third example gets all references though only cmdlets as without it would return external applications too.
{% highlight powershell %}
Get-Command -noun *word*
Get-Command -verb *word*
Get-Command *word* -CommandType cmdlet
{% endhighlight %}



You can pipe output into a range of different outputs
{% highlight powershell %}
command | out-file filename.txt
command | out-gridview
command | out-print
command | Export-Clixml C:filename.xml
{% endhighlight %}

Compare files
{% highlight powershell %}
diff (Compare-object)
{% endhighlight %}

Compare XML of processes to current processes based on name
{% highlight powershell %}
Compare-Object -ReferenaceObject (import-clixml C:filename.xml) -DifferenceObject (Get-Process) -property Name
{% endhighlight %}

Convert to HTML
{% highlight powershell %}
command | convertto-html | out-file test.html
{% endhighlight %}

Snapin's are the old way in Powershell 2. Now it's called Modules.

See what modules are available
{% highlight powershell %}
Get-Module -ListAvailable
{% endhighlight %}

Get commands within a module
{% highlight powershell %}
Get-Command -Module *ModuleName*
{% endhighlight %}

Gets Services On Machine
{% highlight powershell %}
Get-Service
{% endhighlight %}

Columns are trimmed depending on screen size. Can use Get-Member so see what properties are available.

To sort on *propertyName*
{% highlight powershell %}
Command | Sort-Object -Property *PropertyName* -Descending
{% endhighlight %}

Filter to single property and rename it. Useful if piping output into another command to bind on property name. The command to bind, must 'Accept pipeline input' which is visable in the help file
{% highlight powershell %}
COMMAND | Select @{name='MyNewPropertyName'; expression={$_.OldPropteryName}}
{% endhighlight %}

Filter to all properties and rename/add property to another properties value. Useful if piping output into another command to bind on property name
{% highlight powershell %}
COMMAND | Select *,@{name='MyNewPropertyName'; expression={$_.OldPropteryName}}
{% endhighlight %}

If a command doesn't allow piping, you can still provide input, eg. This will get all properties when running Get-AdComputer and only pull the value from the name
{% highlight powershell %}
Get-WMIObject -Class win32_bios -ComputerName (Get-AdComputer -Filter * | select -expand Name)
{% endhighlight %}

Where clause
{% highlight powershell %}
# $_ Allows you to access a row at a time that was passed into the where clause
# $_. Allows you to access a column from the data row
# You don't need to type FilterScript if the first parameter is the filter in {}
Get-Service | Where-Object -FilterScript { $_.Status -eq 'Running' }
Get-Service | Where-Object -FilterScript { $_.Status -eq 'Running' and $_.Name -like '*VMWARE*' }
{% endhighlight %}

Just like in SQL, it's best to reduce the dataset before doing sorting (I.e Ordering in SQL). This reduces the dataset that needs to be sorted as we might not need them. This might be called Filter Left. You should also check to see if the command can filter itself.
{% highlight powershell %}
# Gets services beginning with C
Get-Service -Name C*
{% endhighlight %}

Neat like trick, is powershell knows what kb, mb, gb, tb or pt are. I.e
1000mb / 5mb

You can run scripts or code blocks on remote computers by enabling psremoting
{% highlight powershell %}
Enable-PSRemoting

# For remote commands
invoke-command -scriptblock {} -computer localhost

# For remote script
invoke-command -FilePath H:PowershellScriptsscript1.ps1 -computer localhost

#Throttle connections as will spawn a new powershell instance on your machine too
invoke-command -FilePath H:PowershellScriptsscript1.ps1 -computer localhost,server01,server02 -ThrottleLimtit 2
{% endhighlight %}

Start an interactive session with a remote computer
{% highlight powershell %}
# connect to server01
enter-pssession -computer server01

# Exit
exit-pssession
{% endhighlight %}

Due to kerberos, you cannot start a remote session from a remote session as kerberos wont allow your credentials to be delegated again. Also, you cannot connect to localhost with different credentials, at least for WMI.

Child Jobs
If you run invoke commands on a group of computers, you can break down the output after to view any session.
{% highlight powershell %}
# Returns list of jobs with ID (Parent ID)
Get-Job

# Get sub jobs of parent id 2 and expand each child
Get-Job -id 2 | Select -ExpandProperty childjobs

# Now you've got the list of child id's, you can now retrieve the output
received-job 3

# Now you've got the list of child id's, you can now retrieve the output though this time tell Powershell to keep the output until the parent shell is closed. Else, one read you cannot read it again.
received-job 3 -keep
{% endhighlight %}

WMI Example
{% highlight powershell %}
Get-WMI -Name WIN32_BIOS -Computer server01
{% endhighlight %}


http://www.youtube.com/watch?v=tyjiOn-QBTE&list=PL6D474E721138865A
