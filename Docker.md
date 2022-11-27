
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
sudo docker run -it -rm --name {name} ngnix
```

Shell auf Container starten:
```bash
sudo docker exec -it {NAME} sh
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

User defined Bridge (Isolation):
```bash
sudo docker network create {NAME_OF_BRIDGE}
```

Container User Bridge zuweisen:
```bash
sudo docker run -it --rm --network {NAME_OF_BRIDGE} --name {CONTAINER_NAME} -d {DOCKER_CONTAINER}
```

Container Host zuweisen:
(Container runs directly on HOST without isolation!)
```bash
sudo docker run -it --rm --network host --name {CONTAINER_NAME} -d {DOCKER_CONATINER}
```

Port weiterleiten:

Hostport = Port des Containerhosts => Server
Container = Port des Services im Container
```bash
sudo docker run -itd -rm -p{HOST}:{CONTAINER} -name {NAME} ngnix
```
