# Basic enum

## Import AD modules

```powershell
> Import-Module C:\AD\Tools\ADModule-master\Microsoft.ActiveDirectory.Management.dll
> Import-Module C:\AD\Tools\ADModule-master\ActiveDirectory\ActiveDirectory.psd1
```

## List users

```powershell
> Get-ADUser -Filter *
```

## List and expand property

```powershell
> Get-ADUser -Filter * | Select -ExpandProperty sAMAccountName
> Get-ADUser -Filter * | select -expand sAMAccountName
```

## List groups

```powershell
> Get-ADGroup -Filter *
> Get-ADGroup -Identity 'Domain Admins' -Properties *
> Get-ADGroup -Identity 'Domain Admins' -Properties * | select -expand members
```

## List group members

```powershell
> Get-ADGroupMember -Identity 'Domain Admins'
> Get-ADGroupMember -Identity 'Enterprise Admins'
```

Listing Enterprise Admins only works for a root domain: Enterprise Admins group does not exist in a subdomain.

```powershell
> Get-ADGroupMember -Identity 'Enterprise Admins' -Server rootforest.local
```

## Get Domain Policy

Using Powerview:

<pre class="language-powershell"><code class="lang-powershell"><strong>> . C:\AD\Tools\PowerView.ps1
</strong>> Get-DomainPolicy
> (Get-DomainPolicy).KerberosPolicy
</code></pre>

## List GPO Restricted Groups

Using Powerview:

```powershell
> . C:\AD\Tools\PowerView.ps1
> Get-DomainGPOLocalGroup
GPODisplayName : XXX
GroupName : domain\YYY
GroupMemberOf : { SID }
```

Now we have the list of Restricted Groups of GPO \`XXX\`. We list the members of this group:

```powershell
> Get-DomainGroupMember -Identity YYY
```

## List OUs

```powershell
> Get-DomainOU
```

## List Hybrid & HybridConnect OUs

```powershell
> (Get-DomainOU -Identity Hybrid).distinguishedname | %{Get-DomainObject -SearchBase $_} | select name,samaccounttype
> (Get-DomainOU -Identity HybridConnect).distinguishedname | %{Get-DomainObject -SearchBase $_} | select name,samaccounttype
> Get-ADOrganizationalUnit -Identity 'OU=Hybrid,DC=us,DC=techcorp,DC=local' | %{Get-ADObject -Filter * -SearchBase $_} | select Name,ObjectClass
```

The members of these groups may help compromise Entra ID tenant if compromised.

## List GPOs

```powershell
> Get-DomainGPO
```

### List GPO applying to Group

```powershell
> (Get-DomainOU -Identity HybridConnect).gplink
[LDAP://cn={5269764B-0FBA-43B6-82F7-C1E3B1007FFF},cn=policies,cn=system,DC=us,DC=techcorp,DC=local;0]
> Get-DomainGPO -Identity '{5269764B-0FBA-43B6-82F7-C1E3B1007FFF}'
```

## Enumerate ACL

With PowerView, list modify ACLs for user `XXX`:

```powershell
> Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "XXX"}
```

**This takes a long time, may be better to use BloodHound for this type of enumerations.**

However, BloodHound does not show ACLs on OUs, so it may still be interesting to launch this.
