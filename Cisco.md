
## Switch auf Werkseinstellung

```cisco bash
erase startup-config
```

1.  - If you are using Cisco IOS XE Release 3.6.0E or later releases, enter the erase startup-config privileged EXEC command to clear the contents of your startup configuration.If you are using an earlier release, you can skip this step.
2.  - Press and hold the Mode button. The switch LEDs begin blinking after about 3 seconds.
3.  - Continue holding down the Mode button. The LEDs stop blinking after 7 more seconds, and then the switch restarts.
4.  - The switch now operates like an unconfigured switch. You can enter the switch IP information by using Express Setup as described in the “Running Express Setup” section.


## VLAN's loeschen

```cisco
delete flash:vlan.dat
```

## Aktuelle Konfiguration anzeigen

```cisco
switch>enable
switch#show running-config
switch#show run
```

## Hostname anlegen

```cisco
switch>enable
switch#conf t
switch(config)#hostname {NAME}
```

## Logging anpassen

```cisco
switch>enable
switch#conf t
switch(config)#line con 0
switch(config-line)#logging synchronous
```

## Passwort vergeben und User anlegen

```cisco
switch>enable
switch#conf t
switch(config)#line 0 17
switch(config-line)#password {Passwort}
switch(config-line)#exit
switch(config)#enable secret {Enable-Secret}
switch(config)#username {username} privilege 15 password XXXXXX
switch(config)#service password-encryption
switch(config)#end
switch#wr
```

## VLAN definieren und Ports zuweisen

```cisco
switch>enable
switch#conf t
switch(config)#vlan {VLAN-Nr}
switch(config-vlan)#name {VLAN-Name}
switch(config-vlan)#interface vlan {VLAN-Nr}
switch(config-if)#ip address {IP} {Subnet}
switch(config-if)#no shutdown
switch(config-if)#exit
switch(config)#interface {Modelabhaenging}/0/{PORT}
switch(config-if)#switchport mode access
switch(config-if)#switchport access vlan {VLAN-Nr}
```

## Managment VLAN definieren

```cisco
switch>enable
switch#conf t
switch(config)#vlan 99
switch(config)#interface vlan 99
switch(config-if)#ip address {IP-Mangement} {Subnet-Management}
switch(config-if)#no shutdown
switch(config-if)#exit
```

## VLAN loeschen

```cisco
switch>enable
switch#conf t
switch(config)#no interface vlan {VLAN-Nr}
switch(config)#no vlan {VLAN-Nr}
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

