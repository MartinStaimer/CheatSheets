
User anlegen:
```bash
sudo adduser USERNAME  
```

User anlegen ohne Homeverzeichniss:
```bash
sudo adduser --no-create-home USERNAME
```

User anlegen ohne Schell:
```bash
sudo adduser --shell /bin/false
```

https://wiki.ubuntuusers.de/adduser/

User loeschen:
```bash
sudo deluser USERNAME
```

User und home Ordner loeschen :
```bash
sudo deluser --remove-home USERNAME
```

https://wiki.ubuntuusers.de/deluser/

User Umbenennen:
```bash
sudo usermod -l NEWNAME OLDNAME
```

User einer Gruppe hinzufuegen:
Achtung nur mit -aG Verwenden! Mit -a wird hinzugefügt ohne die anderen Gruppenzugehörigkeiten zu verändern.
```bash
usermod -aG GRUPPE USERNAME
```

Gruppe hinzufügen:
```bash
sudo addgroup GROUPNAME
```

Grupe löschen:
```bash
sudo dellgroup GROUPNAME
```