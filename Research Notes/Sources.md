Links

1. [AD Security: Active Directory Recon Without Admin Rights](https://adsecurity.org/?p=2535)





:book: **https://adsecurity.org/?p=2535**

`[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()`
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
```
Forest:
Domain Controllers
Children
DomainMode
DomainModeLevel
Parent
PdcRoleOwner
RidRoleOwner
InfrastructureRoleOwner
Name

```


`([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()
```
SourceName	TargetName	TrustType	TrustDirection
```

 `(([System.DirectoryServices.ActiveDirectory.Forest]::GetForest((New-Object System.DirectoryServices.ActiveDirectory.DirectoryContext(‘Forest’, $ForestRootDomain)))).GetAllTrustRelationships()`

