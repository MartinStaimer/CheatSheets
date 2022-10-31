
# Hashwert einer Datei berechnen

SHA1 Haschwert
```bash
sha1sum {FILE}
```


SHA256 Haschwert
```bash
sha256sum {FILE}

sha256sum <<< “Teststring“
```

MD5 Haschwert
```bash
md5sum {FILE}
```

## NETCAT Dateien senden

Listener starten:
```bash
nc -l -p {PORT} > {FILE}
```

Sender:
```bash
nc IPListener {PORT} < {FILE}
```

## Kali Tatstaur auf Deutsch umstellen
```bash
setxkbmap -layout de
```

## Filtern von Informationen
per grep (Linux)
```bash
cat {einLogfile.log} | grep {dieZuSuchendeInformation}
```

per Select-String (Windows)
```powershell
type {einLogfile.log} | Select-String {OBJECT}
```

## Microsoft Sysinternals Tool strings (Windows)

https://docs.microsoft.com/en-us/sysinternals/downloads/strings

Mit diesem Tool können Dateien auf „strings“ durchsucht werden.
```cmd
Strings .\BSPDatei.exe

strings .\BSPDatei.exe | findstr /i example*
```

## Kali Workaround Anzeige skalieren

```bash
xrandr --output Virtual-1 --auto
```

## Netcat broker (Linux)

Broker initialisieren:
```bash
ncat -l –broker PORT
```

Auf Subsystem 1:
```bash
ncat {IP Broker} {PORT Broker}
```

Auf Subsystem 2:
```bash
ncat {IP Broker} {PORT Broker}
```

## Lokale Listener mit netstat anzeigen (Linux)

Befehl Kompostion:
```bash
netstat -tlpn
```

oder:

```bash
netstat --tcp --listening --programs --numeric
```

Optionen:
```bash
-t = --tcp
-l = --listening
-p = --programs
-n = --numeric
```

## Zeilen in Datei zaehlen

Befehl Kompostion (Linux):
```bash
cat {Datei} | wc -l
```

Befehl Kompostion (Windows Powershell):
```powershell
type Datei | Measure-Object
```

## Get-WinEvent Eventlogs per Powershell abfragen (Windows)

Alle verfügbaren Logfiles auflisten:

```powershell
Get-WinEvent -ListLog *
```

Doku:

[https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/Get-WinEvent?view=powershell-7.1](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.diagnostics/Get-WinEvent?view=powershell-7.1)

Spezielles Logfile auflisten:
```powershell
Get-WinEvent -ListLog *LOGFILENAME*
```

Setup-Protokoll abrufen:
```powershell
Get-WinEvent -ListLog Setup | Format-List -Property *
```

Alle Ereignisprotokollanbieter abrufen:
```powershell
(Get-WinEvent -ListLog Application).ProviderNames
```
Namen von Ereignisprotokollanbietern abrufen:
```powershell
Get-WinEvent -ListProvider *Policy*
```
Abrufen von Ereignis-IDs, die der Ereignisanbieter generiert:
```powershell

(Get-WinEvent -ListProvider Microsoft-Windows-GroupPolicy).Events | Format-Table Id, Description
```
Abrufen von Ereignissen gefiltetert nach Ereignis-ID:
```powershell
Get-WinEvent -{Path_To_Log} -FilterXPath '*/System/EventID=??‘
```

Doku:
https://learn.microsoft.com/de-de/powershell/module/microsoft.powershell.diagnostics/get-winevent?view=powershell-7.2

https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-xml?view=powershell-7.2

## Http Header untersuchen

Unter Kali oder anderer Linux Distribution:
```bash
curl http://Website -v
```

## Websites untersuchen mit Wappalyzer

https://www.wappalyzer.com

## Seclists bzw. Wordlists unter Kali

`/usr/share/wordlists`

oder installieren:
`sudo apt -y install seclists`

Mit Git Clonen:
`git clone https://github.com/danielmiessler/SecLists.git`

## Subdomain Discovery

`Über crt.sh SSL/TLS Zertifikate suchen, auch hier werden die Subdomains mitaufgelistet:`

https://crt.sh

Über google dorks suche:
`site:*.domain.xxx`

DNS Bruteforce:
`dnsrecon -t brt -d domain.xxx`

Sublist3r:
`git clone https://github.com/aboul3la/Sublist3r.git`
`cd Sublist3r`
`python3 sublist3r -d domain.xxx`

Virtual Hosts
Linux:
`/etc/hosts`

Windows:
`c:\windows\system32\drivers\etc\hosts`
  
FFuF:
`fuff -w /usr/share/seclists/Discovery/DNS/namelist.txt -H “Host:FUZZ.domain.xxx“ -u http://host``_ip_address`

## Cookie Coded Strings

Es können in Cookies Kodierte strings enthalten sein z.B.:
`Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ`

entspicht
`{"id":1,"admin":false}`
 
z.B. Ziel:
`{"id":1,"admin":true}`

dann umcodieren
`eyJpZCI6MSwiYWRtaW4iOnRydWV9`

und austauschen  

Auf www.base64decode.org können solche Kodierten Strings wieder zurückkodiert werden.
  
mit Curl könnten anschließen falsche cookies erstellt werden:

`curl -H "Cookie: session=eyJpZCI6MSwiYWRtaW4iOnRydWV9==; Max-Age=3600;" http://domain.xxx`

## Grep

String in Datei, Log usw. suchen

`grep ausdruck /in/Datei`

Groß Klein – Schreibung ignorieren

`grep -i ausdruck /in/Datei`

Suche Negieren, nur Zeilen ohne Suchbegrif anzeigen

`grep -v ausdruck /in/Datei`

Suche reckursiv durch Verzeichniss

`grep -r -i ausdruck /in/Ornder .`

Suche nach exaktem Ausdruck:

`grep -w -i ausdruck /in/Datei`

Suche nach meherern Ausdrücken:

`grep -E -w -i “ausdruck|ausdruck2“ /in/Datei`