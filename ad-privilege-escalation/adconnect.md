# ADConnect

ADConnect allows synchronisation between an Active Directory domain and an Entra ID tenant. Synchronisation is performed through MSOL accounts that are installed on specific machines.

To list MSOL accounts, see [entra-id-recon.md](../active-directory-enumeration/entra-id-recon.md "mention").

To find password of MSOL account, check in account description the machine on which it is installed and run `ADconnect` from `adconnect.ps1`. Alternatively, with local admin to the machine run mimikatz and dump passwords.

Then, it is possible to [dcsync.md](dcsync.md "mention") because MSOL accounts have replication privileges.
