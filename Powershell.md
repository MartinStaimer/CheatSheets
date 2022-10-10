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

Befehle automatisiert auf mehrern Rechnern per Remoteshell ausführen:

```powershell
type C:\TEST\COMPUTERS.txt

Host1
Host2
Host3

```


```powershell
$computerList = Get-Content C:\Test\Computers.txt | New-PSSession -credentials {CRED}
foreach($computer in $computerList)
{
    {COMMANDS ON REMOTE SYSTEM}
    Remove-PSSession $computer
}
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


## Dateien und Verzeichnisse

Spezielle Dateien ab einem bestimmten Ordner rekursiv mit definiertem Datum in Variable speichern:
```powershell
$items = Get-ChildItem -Path C:\Folder\ -Recurse -include *.pdf | Where-Object {$_.CreationTime -ge "month/day/year hour:minute:second"}

```

Durch Variablen iterieren und z.B. in Zielordner kopieren:
```powershell
foreach($item in $items)
{   
   
   $itempath = $item.DirectoryName + '\' +$item.Name 

   copy-item -Path $itempath -Destination C:\Test\
    
}
```
