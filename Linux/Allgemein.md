
Alle Dateien die zu einem Debian Paket gehören auflisten
```bash
dpkg -L {PAKET NAME}
```

Testen ob oder welches Paket installiert ist
```bash
apt-cache policy {Paket}

#Bsp
apt-cache policy samba
```


Permanentes LOG lesen
```bash
tail -f example.log

# Beenden mit strg+c
```

Mit Cat Datei erzeugen
```bash
cat > {NAME_DER_DATEI} <<{ENDKENNUNG}
lorem
ipsum
dolar
{ENDKENNUNG}

#Bsp
cat > test.txt <<END
lorem
ipsum
dolar
END

```

Hostname ändern
```bash
sudo hostnamectl hostname {NAME}

# Testen mit

sudo hostname -f
# oder
hostnamectl status
# oder
hostnamectl hostname
```

Host aus known_hosts entfernen
```bash
ssh-keygen -R {IP_ADDRESS}
```

ACL Support prüfen
```bash
# Change to root Directory
touch testfile
ls -l testfile
-rw-rw-rw-- .....
setfacl -m g:sudo:rw testfile
ls -l testfile
-rw-rw-r--+ ..... # Should look loke this with + at the End !!!
getfacl testfile
```

Zeit per ntp server einstellen
```bash
sudo apt install ntpdate

sudo ntpdate -b {NTP SERVER}
# z.B.:
sudo ntpdate -b ptbtime2.ptb.de
```

