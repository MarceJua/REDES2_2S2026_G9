
## CONFIGURACIÓN SWITCH MULTICAPA DATACENTER

```cisco
enable
configure terminal
ip routing

interface FastEthernet0/1
 no switchport
 ip address 10.2.9.21 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 10.2.9.25 255.255.255.252
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 10.2.9.0 0.0.0.255
 exit
 
do write
```

## CONFIGURACIÓN SWITCH MULTICAPA PISO 1

```cisco
enable
configure terminal
ip routing

interface FastEthernet0/1
 no switchport
 ip address 10.2.9.1 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 10.2.9.5 255.255.255.252
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 10.2.9.0 0.0.0.255
 exit

do write
```

## CONFIGURACIÓN SWITCH MULTICAPA PISO 2

```cisco
enable
configure terminal
ip routing

interface range FastEthernet0/13 - 16
 no switchport
 channel-protocol lacp
 channel-group 1 mode active
 exit

interface Port-channel 1
 ip address 10.2.9.9 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 no switchport
 ip address 192.198.29.1 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 192.198.29.129 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 192.198.29.0 0.0.0.255
 network 10.2.9.0 0.0.0.255
 exit

do write
```
## CONFIGURACIÓN SWITCH MULTICAPA PISO 3

```cisco
enable
configure terminal
ip routing

interface range FastEthernet0/17 - 20
 no switchport
 channel-protocol lacp
 channel-group 2 mode active
 exit

interface Port-channel 2
 ip address 10.2.9.13 255.255.255.252
 no shutdown
 exit

interface FastEthernet0/1
 no switchport
 ip address 192.198.39.1 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

interface FastEthernet0/2
 no switchport
 ip address 192.198.39.129 255.255.255.128
 ip helper-address 192.198.100.130
 no shutdown
 exit

router eigrp 9
 no auto-summary
 network 192.198.39.0 0.0.0.255
 network 10.2.9.0 0.0.0.255
 exit

do write
```

## CONFIGURACIÓN ROUTER1 (PISO 1) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.2 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.19
 encapsulation dot1Q 19
 ip address 192.198.19.66 255.255.255.240
 ip helper-address 192.198.100.130
 exit

interface GigabitEthernet0/1.29
 encapsulation dot1Q 29
 ip address 192.198.19.2 255.255.255.192
 ip helper-address 192.198.100.130
 exit

router eigrp 9
 network 192.198.19.0 0.0.0.255
 network 10.2.9.0 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER2 (PISO 1) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.6 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.19
 encapsulation dot1Q 19
 ip address 192.198.19.67 255.255.255.240
 ip helper-address 192.198.100.130
 exit

interface GigabitEthernet0/1.29
 encapsulation dot1Q 29
 ip address 192.198.19.3 255.255.255.192
 ip helper-address 192.198.100.130
 exit

router eigrp 9
 network 192.198.19.0 0.0.0.255
 network 10.2.9.4 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER1 (DATACENTER / BIBLIOTECA CENTRAL) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.22 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.39
 encapsulation dot1Q 39
 ip address 192.198.100.3 255.255.255.128
 exit

interface GigabitEthernet0/1.49
 encapsulation dot1Q 49 
 ip address 192.198.100.131 255.255.255.128
 exit

router eigrp 9
 network 192.198.100.0 0.0.0.255
 network 10.2.9.20 0.0.0.3
 exit

do write
```

## CONFIGURACIÓN ROUTER2 (DATACENTER / BIBLIOTECA CENTRAL) 

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.2.9.26 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 no shutdown
 exit

interface GigabitEthernet0/1.39
 encapsulation dot1Q 39
 ip address 192.198.100.4 255.255.255.128
 exit

interface GigabitEthernet0/1.49
 encapsulation dot1Q 49
 ip address 192.198.100.132 255.255.255.128
 exit

router eigrp 9
 network 192.198.100.0 0.0.0.255
 network 10.2.9.24 0.0.0.3
 exit

do write
```


## Configuración de VLANs y LACP (Switch Multicapa Piso 1)
**Objetivo:** Crear las VLANs locales y configurar los enlaces troncales agrupados (EtherChannel) hacia los pisos 2 y 3 utilizando el protocolo LACP en modo activo.

```cisco
enable
configure terminal
vlan 19
 name ADMIN
vlan 29
 name ESTUDIANTES
vlan 39
 name WEB_SERVERS
vlan 49
 name DHCP_SERVERS
exit

interface range FastEthernet0/13 - 16
 no switchport
 channel-protocol lacp
 channel-group 1 mode active
 exit
interface Port-channel 1
 ip address 10.2.9.10 255.255.255.252
 no shutdown
 exit

 interface range FastEthernet0/17 - 20
 no switchport
 channel-protocol lacp
 channel-group 2 mode active
 exit

interface Port-channel 2
 ip address 10.2.9.14 255.255.255.252
 no shutdown
 exit

interface range FastEthernet0/21 - 24
 channel-protocol lacp
 channel-group 3 mode active
 switchport mode trunk
 exit

do write

 ```

## Configuración de Alta Disponibilidad HSRP (Router 1 - Piso 1)
**Objetivo:** Configurar el router principal (Active) para las VLANs 19 y 29, estableciendo una prioridad de 110 y activando preempt para que retome el control automáticamente tras un fallo.
 
 ```cisco
enable
configure terminal

interface GigabitEthernet0/1.19
 standby 19 ip 192.198.19.65
 standby 19 priority 110
 standby 19 preempt
 exit

interface GigabitEthernet0/1.29
 standby 29 ip 192.198.19.1
 standby 29 priority 110
 standby 29 preempt
 exit

 do write
 ```
## Configuración de Alta Disponibilidad HSRP (Router 2 - Piso 1)
**Objetivo:** Configurar el router de respaldo (Standby) para las redes de Administración y Estudiantes, manteniendo la prioridad por defecto (100) para que asuma el control automáticamente solo en caso de que el router principal falle.


  ```cisco
enable
configure terminal

interface GigabitEthernet0/1.19
 standby 19 ip 192.198.19.65
 exit

interface GigabitEthernet0/1.29
 standby 29 ip 192.198.19.1
 exit

do write
 ```

## Configuración de Alta Disponibilidad HSRP (Router 1 - Datacenter)
**Objetivo:** Establecer el router principal (Active) para las redes de los servidores. Se configura con prioridad 110 y la función `preempt` en las VLANs 39 (Web) y 49 (DHCP) para garantizar la disponibilidad continua de los servicios.


  ```cisco
enable
configure terminal

interface GigabitEthernet0/1.39
 standby 39 ip 192.198.100.1
 standby 39 priority 110
 standby 39 preempt
 exit

interface GigabitEthernet0/1.49
 standby 49 ip 192.198.100.129
 standby 49 priority 110
 standby 49 preempt
 exit

do write
 ```

## Configuración de Alta Disponibilidad HSRP (Router 2 - Datacenter)
**Objetivo:** Configurar el equipo de respaldo (Standby) para el Datacenter, utilizando la prioridad por defecto para proteger la conectividad hacia los servidores Web y DHCP en caso de contingencia.


  ```cisco
enable
configure terminal

interface GigabitEthernet0/1.39
 standby 39 ip 192.198.100.1
 exit

interface GigabitEthernet0/1.49
 standby 49 ip 192.198.100.129
 exit

do write
 ```

## Configuración de Capa 2 (Switch de Acceso - Piso 1)
**Objetivo:** Habilitar las VLANs 19 y 29 a nivel local, configurar los enlaces troncales hacia los routers para permitir el paso del tráfico segmentado, y asignar los puertos en modo acceso a las computadoras finales.


  ```cisco
enable
configure terminal

vlan 19
 name ADMIN
vlan 29
 name ESTUDIANTES
exit

interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 exit

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 19
 exit

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 29
 exit

do write
 ```

## Configuración de Capa 2 (Switch de Acceso - Datacenter)
**Objetivo:** Segmentar el tráfico de la Biblioteca Central mediante la creación de las VLANs 39 y 49, habilitar el enlace troncal hacia los routers, y conectar en modo acceso el servidor Web y el servidor DHCP.


  ```cisco
enable
configure terminal

vlan 39
 name WEB_SERVERS
vlan 49
 name DHCP_SERVERS
exit

interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 exit

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 49
 exit

interface FastEthernet0/2
 switchport mode access
 switchport access vlan 39
 exit

do write
 ```

## Configuración de Enlace Troncal LACP (Multicapa Datacenter)
**Objetivo:** Configurar la contraparte del grupo de canales (EtherChannel) hacia el Piso 1 uniendo 4 interfaces físicas mediante el protocolo LACP en modo activo, proveyendo tolerancia a fallos y mayor ancho de banda.

  ```cisco
enable
configure terminal
interface range FastEthernet0/21 - 24
 channel-protocol lacp
 channel-group 3 mode active
 switchport mode trunk
 exit

interface FastEthernet0/1
 no shutdown
 exit
interface FastEthernet0/2
 no shutdown
 exit
do write
 ```
