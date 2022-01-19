Links

1. [AD Security: Active Directory Recon Without Admin Rights](https://adsecurity.org/?p=2535)





:book: [**AD Security: Active Directory Recon Without Admin Rights**](https://adsecurity.org/?p=2535)

*Utilizes ad module*



####  Forest
Forest Information
```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()
```
![This is an image](https://github.com/full-recover/Tutorial-Dump/blob/master/Research%20Notes/Results/AD-Security/GetCurrentForest.png)

Forest Trust
```
$ForestRootDomain = ‘xxx.xxx.com’
```
```
([System.DirectoryServices.ActiveDirectory.Forest]::GetForest((New-Object System.DirectoryServices.ActiveDirectory.DirectoryContext(‘Forest’, $ForestRootDomain)))).GetAllTrustRelationships()
```
Forest Global Catalogs
```
[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest().GlobalCatalogs
```


#### Domain
Domain Information
 ```
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
```
![This is an image](https://github.com/full-recover/Tutorial-Dump/blob/master/Research%20Notes/Results/AD-Security/GetCurrentDomain().png)


Domain Trust

```
([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()
```
![This is an image](https://github.com/full-recover/Tutorial-Dump/blob/master/Research%20Notes/Results/AD-Security/ForestTrustRelationships.png)


Discover Service Accounts

https://github.com/PyroTek3/PowerShell-AD-Recon/blob/master/Find-PSServiceAccounts



