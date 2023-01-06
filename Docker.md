
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

//oder

sudo docker container ls -a
```

Auflisten Container in Betrieb:
```bash
sudo docker ps

//oder 

sudo docker container ls
```

Auflisten vorhandener Images:
```bash
sudo docker images

//oder

sudo docker image list

//oder

sudo docker image ls
```

Container starten mit offenen STD IN:
```bash
sudo docker run -it -rm --name {name} ngnix
```

Shell auf Container starten:
```bash
sudo docker exec -it {NAME} sh

//oder

sudo docker run -it --name {NAME} {IMAGE} /bin/bash

//z.B.:

sudo docker run -it --name testcontainer alpine:3.14 /bin/sh
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

//oder

sudo docker container rm {CONTAINER_ID}
```

Docker Image loeschen
```bash
sudo docker image rm {IMAGE_ID}
```

Docker Container erstellen aber nicht starten
```bash
sudo docker create --name {NAME} {IMAGE}

//z.B.:

sudo docker create -it --name test_create apline:3.14 /bin/sh
```

Docker Container starten
```bash
sudo docker start -ai {NAME}

//z.B.:

sudo docker start -ai test_create
```

Docker Container Logs anzeigen
```bash
sudo docker logs {ID}
```

Docker Container lokal Commiten und als Image bereitstellen
```bash
sudo docker commit {CONTAINER_ID} {REPONAME}/{IMAGENAME}:{VERSION}

//z.B.:

sudo docker commit d75e6909b579 mstaimer/testimage:version1
```

Port weiterleiten:

Hostport = Port des Containerhosts => Server
Container = Port des Services im Container
```bash
sudo docker run -it -rm -p{HOST}:{CONTAINER} --name {NAME} {IMAGE}
```

# Images per .tar austauschen

Docker Image in .tar speichern
```bash
sudo docker save -o {NAME_TARFILE}.tar {TAG_IMAGE}

//z.B.:

sudo docker save -o scriptserver.tar mstaimer/scriptserver:v2
```

.tar öffnen und als image verfügbar machen
```bash
sudo docker load < {NAME_TARFILE}.tar

//z.B.:

sudo docker load < scriptserver.tar 
```


# Datenhandling

## Host mapping

Verzeichniss von Host in Container mapen
```bash
sudo docker run -v {HOSTDIR}:{DOCKERDIR}

//z.B.:

sudo docker run -it -v C:\temp:/home/daten --name test_daten alpine /bin/sh
```

## Datencontainer

Datencontainer erstellen
```bash
sudo create -v {DOCKERDIR} --name {NAME} {IMAGE} true

//z.B.:

sudo docker create -v /home/daten --name datenbox alpine:3.14 true 
```

Datencontainer einbinden
```bash
sudo docker run --volumes-from {CONTAINER_NAME} {IMAGE}

//z.B.:

sudo docker run -it --volumes-from datenbox alpine:3.14 /bin/sh 
```

Volume erstellen und einbinden
```bash
sudo docker volume create {VOLUMENAME}

//z.B.:

sudo docker volume create data

//Zugriff
sudo docker run -v {VOLUMENAME}:{TARGET_PATH_CONTAINER} {IMAGE}

z.B.:
sudo docker run -it -v data:/home/data --name testVol alpine /bin/sh
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

Inspect Bridge:
```bash
sudo docker inspect bridge
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

# Docker Build

Build per Dockerfile:
```bash
sudo docker build -t {PRE_TAG}/{TAG}:{VERSION} .
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

