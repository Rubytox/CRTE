# Entra ID recon

From an Active Directory domain, run the following command to find domain accounts used for synchronization between AD and Entra ID.

```powershell
> Get-ADUser -Filter "samAccountName -like 'MSOL_*'" -Server techcorp.local -Properties * | select samAccountName,Description | fl
```

Note that if the domain is child of a parent domain, the MSOL account may be located in the parent forest.
