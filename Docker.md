
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

Port weiterleiten:

Hostport = Port des Containerhosts => Server
Container = Port des Services im Container
```bash
sudo docker run -itd -rm -p{HOST}:{CONTAINER} -name {NAME} ngnix
```


# Network

Docker Doku:
https://docs.docker.com/network/ipvlan/


Auflistung Netzwerke:
```bash
sudo docker network ls
```

Details => Driver Bridge:
```bash
sudo docker inspect bridge
```

## User Bridge

User defined Bridge (Isolation):
```bash
sudo docker network create {NAME_OF_BRIDGE}
```

Container User Bridge zuweisen:
```bash
sudo docker run -it --rm --network {NAME_OF_BRIDGE} --name {CONTAINER_NAME} -d {DOCKER_CONTAINER}
```

Container Host zuweisen:
(Container runs directly on HOST without isolation like an local applicaton!)
```bash
sudo docker run -it --rm --network host --name {CONTAINER_NAME} -d {DOCKER_CONATINER}
```

## MACVLAN

Container as macvlan:
Alle Container laufen als selbstaendige Netzwerkteilnehme mit IP Addresse im Renchner / Server netzwerk:
```bash
sudo docker network create macvlan --subnet {SUBNET} --gateway {GATEWAY} -o parent={ETHERNET} -d {NETWORKNAME}
```
z.B:
```bash
sudo docker network create macvlan --subnet 192.168.0.0/24 --gateway 192.168.0.1 -o parent=en0s3 -d test_vlan
```

Container in macvlan starten
```bash
sudo docker run -it --rm --network {NETWORKNAME} --ip {IP} --name {CONTAINER_NAME} -d {CONTAINER}
```

Achtung Netzwerkkarte muss auf Promisc mode geschaltet werden:
```bash
sudo ip link set {NAME_KARTE} promisc on
```

## MACVLAN 
Trunkin 

```bash
sudo docker create macvlan \
--subnet {SUBNET} \
--gateway {GATEWAY} \
--o parent={ETHERNET}.20 \
-d macvlan20
```

## IPVLAN (L2)

```bash
sudo docker network create -d ipvlan \
--subnet {SUBNET} \
--gateway {GATEWAY} \
-o parent={NETWORK} \
{NAME_OF_NETWORK}
```

z.B.:
```bash
sudo docker network create -d ipvlan \
--subnet 192.168.0.0/24 \
--gateway 192.168.0.1 \
--o parent=enp0s3 \
myipvlan
```

Container IPVLAN zuweisen:
```bash
sudo docker run -it --rm --network {NAME_OF_IPVLAN} \
--ip {IPADDRESS} \
--name {NAME_OF_CONTAINER} \
-d {CONTAINER}
```

z.B.:
```bash
sudo docker run -it --rm --network myipvlan \
--ip 192.168.0.134 \
--name testWebServer \
-d ngnix

oder:

sudo docker run --net=myipvlan -it --rm alpine
```

## IPVLAN (L3)
Im Router muss eine Statische Route angelegt werden.
Mindesanforderung Linux Kernel v4.2+
``` uname -r``` gibt die Kernelversion aus.



```bash
sudo docker network create -d ipvlan \
--subnet {NEW_SUBNET} \
-o parent={NETWORK_ADAPTER} \
-o ipvlan_mode=l3 \
--subnet {ANOTHER_NEW_SUBNET} \
{NAME_OF_NETWORK}
```

Container in Netzwerk starten:
```bash
sudo docker run -it --rm --network {NAME_OF_NETWORK} \
--ip {ADDRESS_FROM_NEW_SUBNET} \
--name {NAME_OF_CONTAINER} \
-d {CONTAINER}
```


# Docker Compose

Installieren:
```bash
sudo apt install docker-compose -y
```

Start compose:
```bash
mkdir {COMPOSE_NAME}
cd {COMPOSE_NAME}
/{COMPOSE_NAME}$ touch docker-compose.yaml
/{COMPOSE_NAME}$ code .
```

Laufende Compose-Container anzeigen:
```bash
sudo docker-compose ps
```

