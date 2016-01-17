---
layout: post
title: TechEd North America 2014
date: 2014-05-26 21:13
categories: [programming]
tags: [notes, powershell]
---
Below are a few of the sessions from TechEd North America 2014 that I have watched along with a few personal notes. For a complete list of the sessions available head over to <a title="Channel9 MSDN" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/#fbid=" target="_blank">Channel 9</a>.

<strong><a title="A Practical View of Release Management for Visual Studio 2013" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B349#fbid=" target="_blank">A Practical View of Release Management for Visual Studio 2013</a></strong>
<ul>
	<li>Ability to use Powershell DSC within Release Management</li>
	<li>Integrates tightly into TFS although can be used without if desired.</li>
</ul>
<strong><a title="Implementing a Release Pipeline with Release Management for Visual Studio 2013" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B216#fbid=" target="_blank">Implementing a Release Pipeline with Release Management for Visual Studio 2013</a></strong>
<ul>
	<li>Custom tooling can be created and added. I.e exe's, Powershell scripts, etc.</li>
	<li>Pipelines can be automatic or manual.</li>
</ul>
<strong><a style="font-size: 15.555556297302246px;" title="Debugging Tips and Tricks in Visual Studio 2013" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B352#fbid=" target="_blank">Debugging Tips and Tricks in Visual Studio 2013</a></strong>
<ul>
	<li>You can use actually start debugging by using F10 (Step Over) or F11 (Step Into) which will start debugging and will break on the first line of your code. Saves unnecessary break points.</li>
	<li>Going to a particular line and pressing CTRL + F10 (Run To Cursor) will make the debugger goto the certain line. Saves unnecessary break points. If it's not part of the startup path, it will break once that point has been reached.</li>
	<li>When viewing a DataTip, pressing CTRL will make the DataTip windows transparent. This is also true for the IntelliSense window too.</li>
	<li>Enable or Disable Just My Code - Tools &gt; Options &gt; Debugging &gt; General &gt; Just My Code</li>
	<li>Using the [DebuggerNonUserCode()] attribute will hide code from the Call Stack. The only time it will be displayed in the Call Stack window is when 'Show External Code' is enabled.</li>
	<li>You can re-execute a certain block of code without having to restart debugging by using CTRL + Shift + F10 (Set Next Statement) while on the desired line. Note this might causes issues in some scenarios as the code will be run twice.</li>
	<li>When debugging, you can right click code and choose 'Step into Specific' which will give you a selectable list of the multiple function calls. Saves going to definition then using a breakpoint.</li>
	<li>In Visual Studio 2013 the 'Autos' will show the return values of all the calls. Handy for single lines with multiple functions.</li>
	<li>You can write and execute code from within the 'Immediate Window'. This allows code to be tested without having to test all the startup path code too.</li>
	<li>Right clicking on a breakpoint reveals a 'When Hit' option which can be used to write a comment or value to the Output Window.</li>
	<li>In Visual Studio 2012 and above, a new debugger window has been added called 'Parallel Watch'. This is handy when wanting to compare values across multiple recursion calls.</li>
	<li>In Visual Studio 2013 you are now able to see the Call Stack of Async tasks.</li>
	<li>ALT + F12 (Peak Definition) Allows you to peak at the definition without loosing your context. The definition is displayed is displayed in an editor below the selected usage.</li>
</ul>

<strong><a title="Diagnosing Issues in Production Environments with Visual Studio 2013" href="http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B366#fbid=" target="_blank">Diagnosing Issues in Production Environments with Visual Studio 2013</a></strong>
<ul>
	<li>.PDB files are Symbol files. These are required for debugging and the symbol files must match the same build as the binaries not just the same source code version.</li>
	<li>Should archive productions Symbol files.</li>
	<li>Remote debugging in production isn't great as you will be holding up the live systems though sometimes you are forced to do so.</li>
	<li>Remote Debugger needs to run as administrator else it will run under your user credentials which cannot attach to the process. This program needs to run on the server.</li>
	<li>Turning off Just My Code when working with web frameworks like MVC will cause the debugger to not break for user un-handled exceptions as this is a feature/concept of Just My Code. An exception will however be written to the Output Window. Within the Debug menu &gt; Exceptions, you can then set an exception to be thrown rather than logged.</li>
	<li>If you don't have the project open that your debugging, then right click on the blank solution &gt; Properties &gt; Debug Files. Add the paths to the source code. Although, Why not just open the project up?</li>
	<li>If you are remote debugging and your concerned about something taking a while to load which hasn't thrown an exception or broken yet, you can use the pause/breakpoint button in Visual Studio to break on the current executing line.</li>
	<li>Profiler
<ul>
	<li>vsperf..exe - Shows expensive method calls</li>
</ul>
</li>
	<li>IntelliTrace - captures reliability and performance data. Can be used when connecting using a remote debugger ins't available or for when a long sample time is required. IntelliTrace is no longer available as a download for Visual Studio 2013 as it's been moved into the Microsoft Monitoring Agent.</li>
</ul>
