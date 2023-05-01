
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

Verzeichnisse
```bash
/etc/bind
# (named.conf, named.conf.local, named.conf.default-zones, named.conf.options)

/var/cache/bind
# Zonendateien, evtl. log wenn Konfiguriert
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
db.example => /var/cache/bind/db.example (Besitzer ! bind:bind )
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

auch möglich TXT Record (max. 256 Zeichen)
```bash
master IN TXT "Network DNS Server"
```

Bind reverse Zonefile
db.192.168.0 => /var/cache/bind/db.192.168.0 (Besitzer ! bind:bind )
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


Logging ändern (named.conf.options)
```bash
logging {

  channel example_log {
    file "example.log" versions 3 size 250k;   
    severity info;
  };

  category default {
    example_log;
  };
};
```

Sollte jetzt unter "/var/cache/bind/example.log" zu finden sein
https://bind9.readthedocs.io/en/v9_18_4/chapter3.html?highlight=logging#named-conf-base-file

Querylogging 
```bash
# Einschalten
sudo rndc querylog

# Ausschalten -> Wiederhole den Befehl
sudo rndc querylog
```

