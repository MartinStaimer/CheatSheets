
## Switch auf Werkseinstellung

```cisco bash
erase startup-config
```

1.  - If you are using Cisco IOS XE Release 3.6.0E or later releases, enter the erase startup-config privileged EXEC command to clear the contents of your startup configuration.If you are using an earlier release, you can skip this step.
2.  - Press and hold the Mode button. The switch LEDs begin blinking after about 3 seconds.
3.  - Continue holding down the Mode button. The LEDs stop blinking after 7 more seconds, and then the switch restarts.
4.  - The switch now operates like an unconfigured switch. You can enter the switch IP information by using Express Setup as described in the “Running Express Setup” section.

## Model und Version anzeigen

```cisco
switch>show version
```

## Environment 

```cisco
switch#show environment all
```

## Aktuelle Konfiguration anzeigen

```cisco
switch>enable
switch#show running-config
switch#show run
```

## Line Konfiguration anzeigen

```cisco
switch>enable
switch#show running-config | section line
...
switch#show running-config | begin line
```

## Passwort für Console  (line con 0) Konfigurieren

con 0 means Console direct device connection with rollover cable.

```cisco
switch>enable
switch#conf t
switch(config)#line console 0
switch(config-line)#password {PASSWORD}
switch(config-line)#login
```

## Passwort für VTY  (line vty 0 -15) Konfigurieren

```cisco
switch>enable
switch#conf t
switch(config)#line vty 0 15
switch(config-line)#password {PASSWORD}
switch(config-line)#login
```

## SSH für VTY  (line vty 0 -15) Konfigurieren

```cisco
switch>enable
switch#conf t
switch(config)#line vty 0 15
switch(config-line)#transport input ssh
```

## SSH Key generieren

Achtung, vorher muss der Domain- und Hostname vergeben werden, denn er ist teil des Keys.

```cisco
switch>enable
switch#conf t
switch(config)#crypto key generate rsa general-keys modulus {LENGHT}
```

## Hostname anlegen

```cisco
switch>enable
switch#conf t
switch(config)#hostname {NAME}
{NAME}(config)#
```

## Domainname anlegen

```cisco
switch>enable
switch#conf t
switch(config)#hostname {NAME}
{NAME}(config)#
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

## Aktuelle Konfiguration speichern

```cisco
switch>enable
switch#copy running-config startup-config
Destination filename [startup-config]? -> Enter
[OK]
```


# Interfaces

## Mac Table anzeigen

```cisco
switch>enable
switch#show mac address-table
```


## Konfig Interface

```cisco
switch(config)#interface {type/number}
...
switch(config)#exit
```

## List Ports (Interfaces)

```cisco
switch#show ip interface brief
...
switch#sh ip int br
```
Info:
If the numbers look like {Interface}1/0/1, it means that the first number is the stack information. If you have a second switch stacked with the first one, it looks like {Interface}2/0/1



# VLAN

## VLAN's loeschen

```cisco
delete flash:vlan.dat
```

## VLAN's auflisten

```cisco
switch#show vlan
```

## Managment VLAN definieren und Default Gateway

```cisco
switch>enable
switch#conf t
switch(config)#vlan 99
switch(config)#interface vlan 99
switch(config-if)#ip address {IP-Mangement} {Subnet-Management}
switch(config-if)#no shutdown
switch(config-if)#exit
switch(config)#ip default-gateway {GATEWAY_IP}

```


## VLAN definieren und Ports zuweisen

```cisco
switch>enable
switch#conf t
switch(config)#interface GigabitEthernet1/0/1
switch(config-if)#switchport access vlan {NUMBER}
switch(config-if)#exit

switch(config)#vlan {VLAN-Nr}
switch(config-vlan)#name {VLAN-Name}

switch(config)#interface {Modelabhaenging}{Stack}/0/{PORT}
switch(config-if)#switchport mode access
switch(config-if)#switchport access vlan {VLAN-Nr}
```


## VLAN loeschen

```cisco
switch>enable
switch#conf t
switch(config)#no interface vlan {VLAN-Nr}
switch(config)#no vlan {VLAN-Nr}
```


# Trunk

|Mode|Access|Dynamic Auto|Trunk|Dynamic Desirable|
|-------|-------|----------------|------|---------------------|
|access|access|access|!|access|
|dynamic auto|access|access|trunk|trunk|
|trunk|!|trunk|trunk|trunk|
|dynamic desirable|access|trunk|trunk|trunk|




## Trunk Port konfigurieren

```cisco
switch>enable
switch#conf t
switch(config)#
```
