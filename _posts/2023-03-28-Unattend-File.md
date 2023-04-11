---
title: "Unattend.xml File"
date: 2023-03-28
---
### Utilizing the unattend xml file

So, recently I have had the need to utilize an unattend file (Answer file) to get a Windows Operating System to install without the need for user interaction. So, I figured it is a perfect time to throw together some of the things that I utilized in creating my unattend file. I will also throw in the links to any of the applicable things that I found useful as well. In a future blog I will also show how I added the unattend.xml file to the Windows Operating System install.
A good place to start is the Microsoft documentation section on the unattend process, it can spur some definite thoughts on what you may want or need to do.\
&emsp;<a href="https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11">Answer Files</a>\
I found that the majority of what I needed and wanted to do applied to either the 4: Specialize pass or 7: oobeSystem pass. The first thing that you will need is the Windows Assessment and Deployment Kit for the version of Windows you are going to be working on. In my case, it was Windows 10 21H2 that I was specifically working on.\
&emsp;<a href="https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install">Windows Assessment and Deployment Kit</a>\
Once you have the version you need installed you will launch the Windows System Image Manager and the instructions on the answer files Microsoft Learn page will walk you through the process.\

This was a good article that helped me out\
&emsp;<a href="https://www.windowscentral.com/how-create-unattended-media-do-automated-installation-windows-10">Unattend Blog</a>\
As always, a shout out to Mauro Huculak for his post!

<a href="#top">Back to top</a>
---
Updated 3/28/2023\
Link to my <a rel="me" href="https://tech.lgbt/@NathanHamblin_MI6">Mastodon</a>\
Link to my <a rel="me" href="https://www.linkedin.com/in/nathan-hamblin">LinkedIn</a>