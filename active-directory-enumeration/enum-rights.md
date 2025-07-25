# Enum rights

## Enumerate ACL

### With PowerView

List modify ACLs for user `XXX`:

```powershell
> Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "XXX"}
```

\*\*This takes a long time, may be better to use BloodHound for this type of enumerations.\*\*



