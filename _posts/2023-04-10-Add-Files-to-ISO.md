---
title: "Add Files to ISO"
date: 2023-04-10
---
### Add Files to Windows ISO File

A recent thing I had to do with work was to add in some files into an ISO file for Windows.\
The following blog posts were super helpful to adding the files and committing the changes to the ISO.\
The basic overview of what I did to add the files was the following:\
&emsp;1). Create an empty folder on the device and call it Mount, I created mine in <b>C:\Temp\Mount</b> \
&emsp;2). Launch an Administrative PowerShell prompt.\
&emsp;3). Mount the Windows ISO file that you want to work with and add files.\
&emsp;4). I utilized DISM to get the index information about the WIM file I wanted to work with, running <b>DISM /Get-ImageInfo /ImageFile:`<path_to_image.wim>` </b>\
&emsp;5). Grab the index of the Windows version that you want to work with and add files.\
&emsp;6). Mount that index into the C:\Temp\Mount folder by running, <b>Dism /Mount-Image /ImageFile:<path_to_image_file> /Index:`<image_index>` /MountDir:C:\Temp\Mount</b>\
&emsp;7). Once the process runs, you can then copy files into the appropriate locations there, you will need administrative rights to add anything into that mounted image.\
&emsp;8). After getting everything added that you want, you will need to commit the changes and dismount the image by running, <b>Dism /Unmount-Image /MountDir:C:\Temp\Mount /Commit</b>\
This will close up the WIM file, the next process could be to create an ISO with that updated WIM file.

<a href="#top">Back to top</a>
---
Updated 4/10/2023\
Link to my <a rel="me" href="https://tech.lgbt/@NathanHamblin_MI6">Mastodon</a>\
Link to my <a rel="me" href="https://www.linkedin.com/in/nathan-hamblin">LinkedIn</a>