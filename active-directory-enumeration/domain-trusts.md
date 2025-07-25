# Domain trusts

## List domains

```powershell
> (Get-ADForest).Domains
```

## List trusts

```powershell
> Get-ADTrust -Filter *
[PowerView]> Get-ForestDomain -Verbose | Get-DomainTrust | ?{$_.TrustAttributes -eq 'FILTER_SIDS'}
```

Look for "Name" and "Direction". Results also include trust between child and parent domains (e.g. us.techcorp.local and techcorp.local). However it is limited to the current domain, specify `Server` to list trusts of parent domain.

## List all external trusts of root forest

```powershell
> Get-ADTrust -Filter 'intraForest -ne $True' -Server (Get-ADForest).Name
```

_Note: if we have a bidirectional trust with another domain or incoming trust, then we can also extract information from this domain (enumerate trusts, users, ...)_

## List all external trusts for all current domains

```powershell
> (Get-ADForest).Domains | %{Get-ADTrust -Filter '(intraForest -ne $True) -and (ForestTransitive -ne $True)' -Server $_}
```

