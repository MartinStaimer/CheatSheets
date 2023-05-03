
Offene Ports anzeigen:
```bash
// IPv4
netstat -p IP

// TCP
netstat -p tcp

// Sockets
netstat -a

// Anzeige offene Ports und aktive Verbindungen
netstat -ano

// Anzeige offene Ports und zugehöriges Program mit PID
sudo netstat -ltnp

// Offene Ports anzeigen
ss -ntl
# oder mit zugehörigem Prozess
ss -ntle
```