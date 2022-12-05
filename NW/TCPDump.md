
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