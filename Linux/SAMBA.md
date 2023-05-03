
Installation
```bash
sudo apt install -y samba winbind
```

Service Units

smbd.service
nmbd.service
winbind.service
samba-ad-dc.service

## User managment

User Auflisten
```bash
sudo pdbedit --list
```

Ausführlicher (Verbose)
```bash
sudo pdbedit -Lv
```

User anlegen (Achtung vorher LINUX User anlegen!)
```bash
sudo pdbedit -a {NAME}
```

per Script
```bash
printf "%s\n%s\n" {Password} {Password} \
  | sudo pdbedit -a -t {NAME}
```

User loeschen
```bash
sudo pdbedit -x {NAME}
```

Account Flags ändern
```bash
sudo pdbedit -r -u {USERNAME} -c "[FLAGS]"

#Bsp

sudo pdbedit -r -u test -c "[XN]"
```

|Flag|Beschreibung|
|----|------|
|D|Account is disabled.|
|H|A home directory is required.|
|I|An inter-domain trust account.|
|L|Account has been auto-locked.|
|M|An MNS (Microsoft network service) logon account.|
|N|Password not required.|
|S|A server trust account.|
|T|Temporary duplicate account entry.|
|U|A normal user account.|
|W|A workstation trust account.|
|X|Password does not expire.|

https://www.linuxtopia.org/online_books/network_administration_guides/samba_reference_guide/18_passdb_15.html

https://www.samba.org/samba/docs/current/man-html/pdbedit.8.html

Installierte Version herausfinden
```bahs
sudo smbstatus -V
```