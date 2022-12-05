
Show running config
```shell
Router>enable
Router#show running-config
```

Show IP Config
```shell
Router#show ip
```

Environment
```bash
Router#show environment

Router#show environment last

Router#shwo environment table
```

Save Config
```bash
copy running-config startup-config
```

Interface Status
```bash
Router#show interface stats
```

## Router IP Adresse vergeben

```cisco
router>enable
router#conf t
router(config)#interface {Modelabhaengig}
router(config-if)#ip address {IP-Adresse} {Subnet}
router(cinfig-if)#no shutdown
```

## Router Hostname vergeben

```cisco
router>enable
router#conf t
router(config)#hostname {Hostname}
{Hostname}(config)#
```

## Router DNS Lookup im Enable Mode abschalten

```cisco 
router>enable
router#conf t
router(config)#no ip domain-lookup
```

## Router FQDN Konfigurieren

```cisco
router>enable
router#conf t
router(config)#ip domain-name {Domainname}
```

## Router Benutzer anlegen

```cisco
router>enable
router#conf t
router(config)#username {Username} privilege 15 secret {Passwort}
```

## Router SSH Konfigurieren und Crypto Key generieren

```cisco
router>enable
router#conf t
router(config)#line vty 0 4
router(config-line)#transport input ssh
router(config-line)#login local
router(config-line)#exit
router(config)#crypto key generate rsa general-keys modulus 2048
router(config)#ip ssh time-out 60
router(config)#ip ssh authentication-retries 3
router(config)#ip ssh version 2
```

## Router Interface auf LWL Transiver umschalten

```cisco
router>enable
router#conf t
router(config)#interface gigabitEthernet 0/0 (bzw. richtiges Interface ausw.)
router(config-if)#media-type gbic
```

## DHCP

DHCP config
```bash
Router>enable
Router(config)#conf t
Router(config)#ip dhcp excluded-address {START_IP_RANGE} {END_IP_RANGE}
Router(config)#ip dhcp pool {NAME}
Router(dhcp-config)#network {NETWORK} {SUBNET}
Router(dhcp-config)#default-router {Gateway_IP_for_Client}
Router(dhcp-config)#dns-server {DNS_Server_for_Client}
```

## NAT

Show NAT translations and statistic
```bash
Router# show ip nat translations

Router# show ip nat statistics
```

### Config static NAT
```bash
Router#conf t
Router(config)#ip nat inside source static {INTERNAL_IP} {OUTSIDE_IP}
Router(config)#interface {INSIDE_INTERFACE}
Router(congig-if)#ip nat inside
Router(config-if)#interface {OUTSIDE_INTERFACE}
Router(congig-if)#ip nat outside
```

### Dynamic NAT

Earlier you have to implement an ACL for this purpose.

```cisco
Router(config)#ip nat pool {POOL_NAME} {START_IP_POOL} {END_IP_POOL} netmask {SUBNETMASK}

Router(config)#ip nat inside source list {NUMBER} pool {POOL_NAME}
Router(config)#interface {INSIDE_INTERFACE}
Router(congig-if)#ip nat inside
Router(config-if)#interface {OUTSIDE_INTERFACE}
Router(congig-if)#ip nat outside
```

### Dynamic NAT with PAT

Earlier you have to implement an ACL for this purpose.
The difference is the keyword `overload`

```cisco
Router(config)#ip nat pool {POOL_NAME} {START_IP_POOL} {END_IP_POOL} netmask {SUBNETMASK}

Router(config)#ip nat inside source list {ACL_NAME} pool {POOL_NAME} overload
Router(config)#interface {INSIDE_INTERFACE}
Router(congig-if)#ip nat inside
Router(config-if)#interface {OUTSIDE_INTERFACE}
Router(congig-if)#ip nat outside
```

## ACL

```bash
Router(config)#access-list {ACL_NAME} permit {IP_RANGE} {WILDCARD_SUBNET}
```

bsp.: `access-list 1 permit 192.168.0.0 0.0.255.255`
Included all Subnet in 192.168.0.0/16

```Shell
Router(config)#ip access-list standard {NAME}
Router(config-std-nacl)#permit {IP_ADDRESS} {WILDCARD_SUBNETMASK}
Router(config-std-nacl)#permit {IP_ADDRESS} {WILDCARD_SUBNETMASK}
```

Bsp.: `permit 192.168.10.0 0.0.0.255`
