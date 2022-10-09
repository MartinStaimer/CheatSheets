# Powershell


## Remote Shell auf andere Rechner

Sind auf dem Zielrechner Adminrechte vorhanden kann der Zugriff auch Administrativ genutzt werden.

Verbindung herstellen:
```Powershell
New-PSSession -Computername {NAME} -Credential {CRED}
```

Verbindungen anzeigen:
```Powershell
Get-PSSession

Id Name         CompterName   State
-- ----         ----------    -----
1  Connection   Computername  Opened
```

Verbindung oeffnen:
```powershell
Enter-PSSession -Id 1
```

## DateTime Powershell

Variable als DateTime deklarieren:
```powershell
[DateTime]$VariableName
```
Einer DateTime Werte zuweisen:
```powershell
$VariablenName = "10/06/2022 00:00:00"
```
