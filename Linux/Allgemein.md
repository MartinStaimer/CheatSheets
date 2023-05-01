
Alle Dateien die zu einem Debian Paket gehören auflisten
```bash
dpkg -L {PAKET NAME}
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