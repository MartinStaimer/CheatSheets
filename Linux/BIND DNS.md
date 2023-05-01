
### Bind Installieren
```bash
sudo apt install bind9
```

### Version anzeiegen
```bash
named -v
# oder
named -V
```

### Verzeichnisse
```bash
/etc/bind
# (named.conf, named.conf.local, named.conf.default-zones, named.conf.options)

/var/cache/bind
# Zonendateien, evtl. log wenn Konfiguriert
```

### Status BIND 
```bash
sudo systemctl status bind9

# oder

sudo -u bind rndc status
```
bind ist der bind9 Service User


```bash
sudo -u bind rndc-confgen
```

### Allgemeine Konfiguration Prüfen
```bash
sudo named-checkconf
```
Wenn hier kein Output erzeugt wird -> Alles Perfekt 😀

### Zonen Konfiguration Prüfen
```bash
sudo named-checkzone {ZONENAME} {PATH_TO_ZONEFILE}

#Bsp:

sudo named-checkzone localhost /etc/bind/db.local

# oder

named-checkconf -z
```
Hier wird auf jeden Fall eine Ausgabe erzeugt -> wenn alles passt -> OK am Ende des Outputs.

### Nützliche Prüfung des DNS
```bash
dig -t A {dns_name} @127.0.0.1
```

Nützliches Python Paket (Dnspython)
https://dnspython.readthedocs.io/en/latest/manual.html

### Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "example.com" IN {
type master;
file "db.example";
allow-update { none; };
};
```

### Reverse Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "0.168.192.in-addr-arpa" IN {
type master;
file "db.192.168.0";
allow-update { none; };
};
```

#### Bind Zonefile
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

##### auch möglich TXT Record (max. 256 Zeichen)
```bash
master IN TXT "Network DNS Server"
```

##### Automatisch Generierte DNS Namen
```bash
$GENERATE {IP_START-IP_END} {NAME}-$ IN A {IP_FIST_3_OCTETS}.$

#Bsp

$GENERATE 20-50 dhcp-$1 IN A 192.168.0.$
```

#### Bind reverse Zonefile
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


### Logging ändern (named.conf.options)
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

### Querylogging 
```bash
# Einschalten
sudo rndc querylog

# Ausschalten -> Wiederhole den Befehl
sudo rndc querylog
```


### Slave DNS and Zonetransfer

Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "example.com" IN {
       type slave;
       file "db.example";
       masters {IP_OF_MASTER; };
};
```

Reverse Zone hinzufuegen (Eintrag in named.conf.local)
```bash
zone "0.168.192.in-addr-arpa" IN {
       type slave;
       file "db.192.168.0";
       masters {IP_OF_MASTER; };
};
```
Der Addressbereich "192.168.0" ist nur ein Beispiel hier könnte auch "10.0.5" stehen.

##### In der db.example den Slave eintragen!

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
@         IN NS slave.example.com.
master    IN A  192.168.0.5
slave     IN A  192.168.0.6

; Aliases
ns1       IN CNAME  master.example.com.
ns2       IN CNAME  slave.example.com.
```

##### Manuell Retransfer anstoßen
```bash
rndc retransfer example.com
rndc retransfer 0.168.192.in-addr-arpa
```

### Security

In named.conf.local => Einzufuegen bei der Zonenkonfiguration,
oder Allgemein in der named.conf.options
```bash
allow-transfer { none; };
```
 => Wenn kein slave existiert.

Dynamic Update aus
```bash
allow-update { none; };
```
https://bind9.readthedocs.io/en/v9_18_4/chapter6.html?highlight=allow-update#dynamic-update-security

Query Zugriff steuern:
In named.conf.options
```bash
allow-query { localhost; Known Subnet; 192.168.0.0/24;}
```

Wenn ein Slave System installiert wird sollte das TSIG Verfahren angwendet werden.
Es basiert auf Public- und Privatekey Komunikation.
https://bind9.readthedocs.io/en/v9_18_4/chapter6.html?highlight=TSIG#tsig

### DNSSEC (Achtung ist evtl. veraltet)

Zu named.conf.options folgende Einträge hinzufügen
```bash
dnssec-validation auto;
```

Im Verzeichniss /var/cache/bind key generieren
```bash
dnssec-keygen -a ECDSAP256SHA256 -n ZONE example.com

#anschließend

dnssec-keygen -f KSK -a ECDSAP256SHA256 -n ZONE example.com
```
https://bind9.readthedocs.io/en/v9_18_4/chapter7.html?highlight=dnssec-keygen#generating-keys

Jetzt die "Public Key" Dateien an die Zonendatei mit $INCLUDE anhängen
```bash
$INCLUDE Kexample.com.+013+39444.key
$INCLUDE Kexample.com.+013+64979.key
```

Serial erhöhen !

Signed ZONE erstellen
```bash
dnssec-signzone -o example.com db.example
```
Das muss wohl bei jeder Änderung in der db.example bzw. der Zonendatei ausgeführt werden!


Jetzt in der named.conf.local den file Eintrag gegen db.example.signed tauschen
```bash
zone "example.com" {
     file "db.example";
}
 
# Gegen

zone "example.com" {
     file "db.example.signed";
}
```

https://bind9.readthedocs.io/en/v9_18_4/chapter7.html?highlight=dnssec#dnssec

