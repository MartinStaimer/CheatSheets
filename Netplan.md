	
Path Ubuntu:
`/etc/netplan/00-installer-config.yaml`

```bash
sudo netplan apply
```


## DHCP

```yaml
network:
    version: 2
    renderer: networkd
    ethernets:
        enp1s0:
            dhcp4: true
```


## STATIC IP

```yaml
network:
    version: 2
    renderer: networkd
    ethernets:
        enp1s0:
            addresses:
                - 192.168.1.2/24
            nameservers:
                search: [mydomain, otherdomain]
                addresses: [192.168.1.250, 1.1.1.1]
            routes:
                - to: default
                  via: 192.168.1.1
```


Mehr Beispiele:
https://netplan.io/examples/