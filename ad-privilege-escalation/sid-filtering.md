# SID Filtering

## List groups with SID > 1000

```powershell
> Get-ADGroup -Filter 'SID -ge "S-1-5-21-4066061358-3942393892-617142613-1000"' -Server euvendor.local
```
