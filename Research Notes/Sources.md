Links

1. [AD Security: Active Directory Recon Without Admin Rights](https://adsecurity.org/?p=2535)





:book: [**AD Security: Active Directory Recon Without Admin Rights**](https://adsecurity.org/?p=2535)

`[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()`
![This is an image](https://github.com/full-recover/Tutorial-Dump/blob/master/Research%20Notes/Results/AD-Security/GetCurrentForest.png)

```
Name
Sites
Domains
GlobalCatalogs
Application Partitions
ForestModeLevel
ForestMode
RootDomain
Schema
SchemaRoleOwner
NamingRoleOwner

```
 `[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()`
![This is an image](https://github.com/full-recover/Tutorial-Dump/blob/master/Research%20Notes/Results/AD-Security/GetCurrentDomain().png)
```

```


`([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()`
```
SourceName	TargetName	TrustType	TrustDirection
```

 `(([System.DirectoryServices.ActiveDirectory.Forest]::GetForest((New-Object System.DirectoryServices.ActiveDirectory.DirectoryContext(‘Forest’, $ForestRootDomain)))).GetAllTrustRelationships()`

