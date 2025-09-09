# Methodology

Example of AD pentest methodology for CRTE:

1. Local privilege escalation on exam machine: [recon.md](local-privilege-escalation/recon.md "mention")
2. AD Enumeration:
   1. BloodHound with several user accounts during the tests: [sharphound.md](active-directory-enumeration/sharphound.md "mention")
   2. Starting domain enumeration: [todo.md](active-directory-enumeration/todo.md "mention")
   3. List trusts: [domain-trusts.md](active-directory-enumeration/domain-trusts.md "mention")
   4. Domain enumeration on trusts: [todo.md](active-directory-enumeration/todo.md "mention")
   5. List MSOL accounts: [entra-id-recon.md](active-directory-enumeration/entra-id-recon.md "mention")
3. Privilege escalation on starting domain:
   1. Find kerberoastable users: [kerberoasting.md](ad-privilege-escalation/kerberoasting.md "mention")
   2. Find machines where LAPS is readable by a compromised user: [laps.md](ad-privilege-escalation/laps.md "mention")
   3. Find gMSA accounts whose passwords are readable by a compromised user: [gmsa.md](ad-privilege-escalation/gmsa.md "mention")
   4. List UD servers [unconstrained-delegation.md](ad-privilege-escalation/unconstrained-delegation.md "mention") and DCSync [dcsync.md](ad-privilege-escalation/dcsync.md "mention")
   5. List devices that support CD [constrained-delegation.md](ad-privilege-escalation/constrained-delegation.md "mention") and impersonate user
   6. If a user has GenericWrite on machine account, use [rbcd.md](ad-privilege-escalation/rbcd.md "mention") to get machine account and then local admin with [s4u.md](ad-privilege-escalation/s4u.md "mention")
   7. If a user has DCSync, then [dcsync.md](ad-privilege-escalation/dcsync.md "mention")
   8. If I am local admin of a MSOL server, read password [adconnect.md](ad-privilege-escalation/adconnect.md "mention") and [dcsync.md](ad-privilege-escalation/dcsync.md "mention")
   9. If there is an MSSQL server, list accesses and SQL links: [sql-servers.md](ad-privilege-escalation/sql-servers.md "mention")
   10. If I am local admin of a server, dump LSASS with [lsass.md](ad-privilege-escalation/lsass.md "mention")
   11. If I am local admin of a server, decrypt secrets (certificates for instance) with [dpapi.md](ad-privilege-escalation/dpapi.md "mention")
   12. ADCS: [Broken link](broken-reference "mention")
4. Lateralise to parent forest:
   1. Golden ticket with krbtgt: [golden-ticket.md](ad-privilege-escalation/golden-ticket.md "mention")
   2. Domain trust key and silver ticket: [da-to-ea.md](ad-privilege-escalation/da-to-ea.md "mention")
   3. Perform similar checks as for starting domain
5. Lateralise to trusted forest:
   1. Domain trust key and silver ticket: [da-to-ea.md](ad-privilege-escalation/da-to-ea.md "mention")
   2. SID filtering: [sid-filtering.md](ad-privilege-escalation/sid-filtering.md "mention")

