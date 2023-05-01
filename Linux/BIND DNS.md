
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

# oder

named-checkconf -z
```
Hier wird auf jeden Fall eine Ausgabe erzeugt -> wenn alles passt -> OK am Ende des Outputs.

Nützliche Prüfung des DNS
```bash
dig -t A {dns_name} @127.0.0.1
```

Nützliches Python Paket (Dnspython)
https://dnspython.readthedocs.io/en/latest/manual.html

Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "example.com" IN {
type master;
file "db.example";
allow-update { none; };
};
```

Reverse Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "0.168.192.in-addr-arpa" IN {
type master;
file "db.192.168.0";
allow-update { none; };
};
```

Bind Zonefile
db.example
```bash
$TTL 3h
@    IN   SOA   master.example.com. root.example.com. (
          01 ; Serial (Bestpractice => DATUMNR => 2023050101)
          8h ; Refresh
          4h ; Retry
          1w ; Expire
          1h ; Negative TTL
)

@         IN NS master.example.com.
master    IN A  192.168.0.5

; Aliases
ns1       IN CNAME  master.example.com.
```

Bind reverse Zonefile
db.192.168.0
```bash
$TTL 3h
@    IN   SOA   master.example.com. root.example.com. (
          01 ; Serial (Bestpractice => DATUMNR => 2023050101)
          8h ; Refresh
          4h ; Retry
          1w ; Expire
          1h ; Negative TTL
)

@         IN NS   master.example.com.
5         IN PTR  master.example.com.

```