Start Server from idrac with commands -> SSH Shell

| Befehl                          | Wirkung                         |
| ------------------------------- | ------------------------------- |
| `racadm serveraction powerup`   | Server einschalten              |
| `racadm serveraction powerdown` | Server ausschalten              |
| `racadm serveraction hardreset` | Hard Reset (Neustart erzwingen) |
| `racadm serveraction softreset` | Soft Reset (sauberer Neustart)  |
| `racadm serveraction status`    | Status des Servers abfragen     |


Systeminformationen

#Hardware-Status anzeigen
racadm getsysinfo

#Sensorenstatus auslesen
racadm getsel

#Alle Systeminformationen
racadm getsensorinfo

