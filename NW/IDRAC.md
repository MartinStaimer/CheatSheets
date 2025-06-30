Start Server from idrac with commands -> SSH Shell

| Befehl                          | Wirkung                         |
| ------------------------------- | ------------------------------- |
| `racadm serveraction powerup`   | Server einschalten              |
| `racadm serveraction powerdown` | Server ausschalten              |
| `racadm serveraction hardreset` | Hard Reset (Neustart erzwingen) |
| `racadm serveraction softreset` | Soft Reset (sauberer Neustart)  |


Systeminformationen

#Hardware-Status anzeigen
```bash
racadm getsysinfo
```

#Sensorenstatus auslesen
```bash
racadm getsel
```

#Alle Systeminformationen
```bash
racadm getsensorinfo
```
