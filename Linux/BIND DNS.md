
Bind Installieren
```bash
sudo apt install bind9
```

Version anzeiegen
```bash
named -v
# oder
named -V
```

Verzeichniss
```bash
/etc/bind
```

Status BIND 
```bash
sudo systemctl status bind9

# oder

sudo -u bind rndc status
```
bind ist der bind9 Service User


```bash
sudo -u bind rndc-confgen
```

Allgemeine Konfiguration Prüfen
```bash
sudo named-checkconf
```
Wenn hier kein Output erzeugt wird -> Alles Perfekt 😀

Zonen Konfiguration Prüfen
```bash
sudo named-checkzone {ZONENAME} {PATH_TO_ZONEFILE}

#Bsp:

sudo named-checkzone localhost /etc/bind/db.local
```
Hier wird auf jeden Fall eine Ausgabe erzeugt -> wenn alles passt -> OK am Ende des Outputs.

Nützliche Prüfung des DNS
```bash
dig -t A {dns_name} @127.0.0.1
```
