
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

Commands auf Remotesystem über PSSession:

```powershell
Invoke-Command -Session {PSSESSION} -SctiptBlock { {auszufuhrende Powershell Befehle} }
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

## Custom Datatypes

```powershell
class Units
{
    [ValidateNotNullOrEmpty()][string]$days
    [ValidateNotNullOrEmpty()][string]$path

    Units($Days, $Path){
        $this.days = $Days
        $this.path = $Path
    }
}


$test = [Units]@{
    Days = "30"
    Path = "/ctre/"
}


// BSP without Constructor
class Units
{
    [ValidateNotNullOrEmpty()][string]$days
    [ValidateNotNullOrEmpty()][string]$path
    
}

$test = New-Object Units[] 10

$test[1] = [Units]@{
              days = "30"
              path = "/test"
}

$test[2] = [Units]@{
              days = "5"
              path = "/test/2"
}

foreach ($destination in $test)
{
    echo $destination.days
    echo $destination.path

}
```
