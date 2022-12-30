
## Auflisten verfügbarer Interfaces
```bash
sudo tcpdumo --list-interfaces
```

oder:

```bash
ip addr show
```


## Lesen Netzwerkverkehr und schreiben in PCAP

```bash
sudo tcpdump -i {INTERFACE} -w {NAMEOFFILE}.pcap
```

Diese kann mit Wireshark geöffnet werden.

## Integrierte Protokolle

arp, fddi, icmp, ip, ip6, rarp, tcp, udp

```bash
sudo tcpdump {PROTOKOLL}
```


## Oeffnen pcap

```bash
wireshark {NAMEOFFILE}.pcap
```

## Spez Port

```bash
tcpdump -nnSX port {PORTNUMBER}
```

Options:

|Parameter|Beschreibung|
|----------------|------------------------------|
|-nn| Löse Hostnamen nicht auf|
|-S| Hole das gesammte Packet|
|-X| Gib in HEX Format aus|



## Spez Host

```bash
tcpdump host {IP_ADDRESS}

//Source
tcpdump src {IP_ADDRESS}

//Destination
tcpdump dst {IP_ADDRESS}

//Net
tcpdump net {IP_ADDRESS/CIDR}

//Portrange
tcpdump portrange {START_PORTNUMBER}-{END_PORTNUMBER}
```

