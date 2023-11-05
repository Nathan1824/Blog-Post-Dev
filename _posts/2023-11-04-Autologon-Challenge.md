---
title: "Autologon"
date: 2023-11-04
---
# Trouble with Autologon
Just in case it can help someone else, I had an interesting issue arise while trying to utilize the Sysinternals Autologon on some Windows 10 LTSC 21H2 devices.\
The issue that I was having is that despite setting the account successfully and rebooting the device, the device would not log in with the account. Upon further investigating I saw the **AutoLogonCount** registry key under the following registry key.

**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon**\
Value Name: **AutoLogonCount**\
Type: **REG_SZ**\
Value: **0**

That key was something I had been using during deployment and that had not cleared out post-imaging. So, the solution was removing that registry key for **AutoLogonCount** prior to running the Sysinternals AutoLogon utility.
To remove the registry key I utilized PowerShell, the first step in writing the code was to detect if the value existed with a simple test.
```
$ALCRegKeyExists = Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "AutoLogonCount" -ErrorAction SilentlyContinue

if(-not [string]::IsNullOrEmpty($ALCRegKeyExists)){
	Write-Host “The registry key AutoLogonCount exists”
	try{
		Remove-ItemProperty -Path “HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name “AutoLogonCount” -Force
		Write-Host “Removed the registry key AutoLogonCount”
	}

	catch{
		Write-Host “There was an error: $($Error[0].Exception)”
	}
}
```

After successfully setting the account with Autologon and rebooting the machine, the device did log in as expected with the autologon account.

For more information on <a href="https://learn.microsoft.com/en-us/sysinternals/downloads/autologon">Sysinternals Autologon</a>

Another key that I needed to set for the environment that I was working with was setting the **ForceAutoLogon** value. This will set the device to re-login with the autologon account if someone tries to log off the device.

**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon**\
Value Name: **ForceAutoLogon**\
Type: **REG_DWORD**\
Value: **1**

I utilized the similar PowerShell scripting to set the key and value.
```
$FALRegKeyExists = Get-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "ForceAutoLogon" -ErrorAction SilentlyContinue

if([string]::IsNullOrEmpty($FALRegKeyExists)){
	Write-Host “The registry key ForceAutoLogon does not exist”
	try{
		New-ItemProperty -Path “HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name “ForceAutoLogon”  -PropertyType DWord -Value 1
		Write-Host “Created the registry key for ForceAutoLogon”
	}

	catch{
		Write-Host “There was an error: $($Error[0].Exception)”
	}
}
```
If needed, you can utilize an **elseif** statement if the value is not set to 1.
```
elseif($FALRegKeyExists.ForceAutoLogon -ne 1){
	Write-Host “The registry key exists but the value is not set to 1”
	try{
		Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "ForceAutoLogon" -Value 1
	}

	catch{
		Write-Host “There was an error: $($Error[0].Exception)”
	}
}
```
The other thing that I ran into in the environment that I am working in is that the network was not connecting immediately. So, I also set a key to wait for the network to connect before attempting to log on. In the registry, I found that setting the **SyncForegroundPolicy** value has the device wait before attempting to log in.

**HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows NT\CurrentVersion\Winlogon**\
Value Name: **SyncForegroundPolicy**\
Type: **REG_DWORD**\
Value: **1**

Using PowerShell to set the registry key for **SyncForegroundPolicy** with the following.
```
$SFPRegKeyExists = Get-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "SyncForegroundPolicy" -ErrorAction SilentlyContinue
if([string]::IsNullOrEmpty($SFPRegKeyExists)){
	Write-Host “The registry key SyncForegroundPolicy does not exist”
	try{
		New-Item -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\CurrentVersion\Winlogon" -Force
		New-ItemProperty -Path “HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name “SyncForegroundPolicy”  -PropertyType DWord -Value 1
		Write-Host “Created the registry key for SyncForegroundPolicy”
	}

	catch{
		Write-Host “There was an error: $($Error[0].Exception)”
	}
}
```
If needed, you can utilize **elseif** statement if the value is not set to 1.
```
elseif($SFPRegKeyExists.SyncForegroundPolicy -ne 1){
	Write-Host “The registry key exists but the value is not set to 1”
	try{
		Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\CurrentVersion\Winlogon" -Name "SyncForegroundPolicy" -Value 1
	}

	catch{
		Write-Host “There was an error: $($Error[0].Exception)”
	}
}
```
---
Updated 11/4/2023\
Link to my <a rel="me" href="https://tech.lgbt/@NathanHamblin_MI6">Mastodon</a>\
Link to my <a rel="me" href="https://www.linkedin.com/in/nathan-hamblin">LinkedIn</a>\
Link to my <a href="https://twitter.com/NathanHamblin8" rel="me">Twitter</a>