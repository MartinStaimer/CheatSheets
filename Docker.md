sud
# Installation

```bash
sudo apt update
sudo apt install docker.io -y
```

# Betrieb

Auflisten aller Container:
```bash
sudo docker ps --all

sudo docker ps -a
```

Auflisten Container in Betrieb:
```bash
sudo docker ps
```

Container starten mit offenen STD IN:
```bash
sudo docker run -it -rm -name {name} ngnix
```

Shell auf Container starten:
```bash
sudo docker exec -it {NAME} sh
```

Container automatisch mit Rechnerhochlauf starten
```bash
sudo docker run --restart always
```

Docker container stoppen
```bash
sudo docker stop {NAME}
```

Docker Container loeschen
```bash
sudo docker rm  {NAME}
```

# Network

Auflistung Netzwerke:
```bash
sudo docker network ls
```

Details => Driver Bridge:
```bash
sudo docker inspect bridge
```

Port weiterleiten:

Hostport = Port des Containerhosts => Server
Container = Port des Services im Container
```bash
sudo docker run -itd -rm -p{HOST}:{CONTAINER} -name {NAME} ngnix
```
