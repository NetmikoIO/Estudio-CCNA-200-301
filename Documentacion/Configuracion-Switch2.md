
<img src="../Imagenes/cisco.svg" alt="Logo Cisco" width="100">

# **Cisco IOS Sheet**  

## Índice  
1. [Cambiar de nombre al dispositivo](#1cambiar-de-nombre-al-dispositivo)  
   - [Resetear el nombre del dispositivo](#11-resetear-el-nombre-del-dispositivo)  
2. [Proteger EXEC privilegiado](#2proteger-exec-privilegiado)  
3. [Proteger acceso por consola](#3proteger-acceso-por-consola)  
4. [Backup de la configuración actual](#4backup-de-la-configuracion-actual)  
5. [Configurar interfaz virtual (SVI)](#5configurar-interfaz-virtual-svi)  
6. [Configurar acceso telnet](#6configurar-acceso-telnet)  
   - [Conectarse desde otro dispositivo](#61-conectarse-desde-otro-dispositivo)  
7. [Configurar DHCP en un router/switch](#7configurar-dhcp-en-un-routerswitch)  
   - [Para un pool llamado smrPool](#71-para-un-pool-llamado-smrpool)  
   - [Para activarlo](#72-para-activarlo)  
8. [Ver la tabla MAC de un switch](#8ver-la-tabla-mac-de-un-switch)  
9. [Selección de interfaces](#9seleccion-de-interfaces)  
   - [Para rango de puertos](#91-para-rango-de-puertos)  
10. [Añadir una descripción a la interfaz](#10-añadir-una-direccion-a-la-interfaz)  
11. [Especificar la velocidad de los puertos](#11-especificar-la-velocidad-de-los-puertos)  
12. [Especificar el modo de comunicación](#12-especificar-el-modo-de-comunicacion)  
13. [Creación de VLANs](#13-creacion-de-vlans)  
14. [Interfaz en modo trunk](#14-interfaz-en-modo-trunk)  
15. [Poner interfaz en modo access](#15-poner-interfaz-en-modo-access)  
16. [Asignar una interfaz a una VLAN](#16-asignar-una-interfaz-a-una-vlan)  
17. [Ver bases de datos de VLANs](#17-ver-bases-de-datos-de-vlans)  

---

# **Operativa con VTP**  
1. [Designamos servidor VTP](#designamos-servidor-vtp)  
2. [Configuramos un dominio VTP](#en-el-servidor-vtp-creamos-un-dominio-vtp-nombre-y-contraseña)  
3. [Designamos clientes VTP](#designamos-que-switches-seran-clientes-vtp)  
4. [Inscribimos clientes al dominio VTP](#inscribimos-los-clientes-al-dominio-vtp)  
5. [Crear VLANs desde el servidor VTP](#desde-el-servidor-vtp-se-crean-las-vlan)  
6. [Configurar un switch transparente](#configurar-un-switch-transparente)  
7. [Ver el estado de un dominio VTP](#ver-el-estado-de-un-dominio-vtp)  

---

# **Enrutado entre VLANs con un switch multicapa**  
1. [Crear las VLANs](#creamos-las-2-vlans)  
2. [Configurar interfaces en modo acceso](#ponemos-las-interfaces-necesarias-en-modo-de-accesso-y-las-asociamos-a-las-vlan)  
3. [Configurar SVI (Switch Virtual Interfaces)](#configuramos-las-svi-switch-virtual-interfaces-en-el-switch)  
4. [Activar enrutado en el switch](#activamos-enrutado-en-el-switch)  

---

# **Cosas para Routers**  
1. [Poner una IP a una interfaz](#poner-una-ip-a-una-interfaz)  
   - [Caso 1: Interfaz Ethernet](#caso-1-interfaz-ethernet-difusion)  
   - [Caso 2: Interfaz Serial](#caso-2-interfaz-serial-punto-a-punto)  
     - [Parte DCE (la del reloj)](#parte-dce-la-del-reloj)  
     - [Parte DTE](#parte-dte)  
2. [Ver tabla de rutas](#ver-tabla-de-rutas)  
3. [Añadir ruta estática](#añadir-ruta-estatica)  

# **Cisco IOS Sheet** 
## 1.Cambiar de nombre al dispositivo 
```
sw(config)# hostname nuevo_nombre
```
### 1.1 Resetear el nombre del dispositivo
```
sw(config)# no hostname
```
## 2.Proteger EXEC privilegiado
```
sw(config)# enable secret contraseña
```
## 3.Proteger acceso por consola 
```
sw(config)# line console 0
sw(config-line)# password mi_contraseña
sw(config-line)# login
```
## 4.Backup de la configuracion actual 
```
sw# copy running-config startup-config
```
## 5.Configurar interfaz virtual (SVI)
```
sw(config)# interface vlan 1
sw(config-if)# ip address DIRECCION_IP MASCARA_DE_RED
sw(config-if)# no shutdown
```
## 6.Configurar acceso telnet
```
sw(config)# line vty 0 15
sw(config-line)# password contraseña
sw(config-line)# login
sw(config-line)# transport input telnet
```
### 6.1 Conectarse desde otro dispositivo: 
```
PC>telnet
Trying 192.168.1.1 ...Open
```
## 7.Configurar DHCP en un router/switch
### 7.1 Para un pool llamado smrPool:
```
sw(config)#ip dhcp pool smrpool
sw(dhcp-config)#network 192.168.1.0 255.255.255.0
sw(dhcp-config)#default-router 192.168.1.1
sw(dhcp-config)# dns-server 8.8.8.8
sw(dhcp-config)#exit
sw(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.10
```
### 7.2 Para activarlo 
```
sw(config)# service dhcp
```
## 8.Ver la tabla MAC de un switch
```
sw# show mac-address-table
```
## 9.Selección de interfaces
```
sw(config)# interface [type] [module/number]
```
>  **[type]**: FastEthernet, GigabitEthernet
>  **[module/number]** :0/1, 0/2, 1/1 ...
>> Ejemplo : sw(config)# interface FastEthernet 0/1
### 9.1 Para rango de puertos:
```
sw(config)# interface range FastEthernet 0/1-4, FastEthernet 0/10, FastEthernet 0/20-24
```
## 10. Añadir una dirección a la interfaz
```
sw(config-if)# description ´interfaz sala 1´
```
## 11. Especificar la velocidad de los puertos
```
sw(config-if)# speed [10 | 100 | 1000 | auto]
```
## 12. Especificar el modo de comunicación
```
sw(config-if)# duplex [auto | full | half]
```
## 13. Creacion de VLANs
```
sw(config)# vlan número
sw(config-vlan)# name mi_vlan_de_prueba
```
## 14. Interfaz en modo trunk
```
sw(config)# interface FastEthernet 0/1
sw(config-if)# switchport mode trunk
```
## 15. Poner interfaz en modo access
```
sw(config)# interface FastEthernet 0/1
sw(config-if)# switchport mode access
```
## 16. Asignar una interfaz a una VLAN
> Requiere hacer 1º (poner interfaz en modo access)
```
sw(config-if)# switchport access vlan numero
```
## 17. Ver bases de datos de VLANs
```
sw# show vlan
```
***
# Operativa con VTP
1. Designamos servidor VTP
2. En el servidor VTP creamos un dominio VTP (nombre y contraseña)
```
servidor-vtp(config)# vtp mode server
servidor-vtp(config)# vtp domain 1smr
servidor-vtp(config)# vtp password 123456
```
3. Designamos que switches seran clientes VTP
```
vtp-cliente-1(config)# vtp mode client
```
4. Inscribimos los clientes al dominio VTP
```
vtp-cliente-1(config)# vtp domain 1smr
vtp-cliente-1(config)# vtp password 123456
```
5. Desde el servidor VTP se crean las VLAN
## Configurar un switch transparente
```
sw-transparente(config)# vtp mode transparent
```
## Ver el estado de un dominio VTP
```
vtp-cliente-1# show vtp status
```
## Enrutado entre VLANs usando un switch multicapa
1. Creamos las 2 VLANs
2. Ponemos las interfaces necesarias en modo de accesso y las asociamos a las VLAN
3. Configuramos las SVI (Switch virtual interfaces) en el switch
```
sw-multicapa(config)# interface vlan X
sw-multicapa(config-if)# ip address 192.168.10.1 255.255.255.0
sw-multicapa(config-if)# no shutdown
sw-multicapa(config-if)# exit
sw-multicapa(config)# inteface vlan y
sw-multicapa(config-if)# ip address 192.168.20.1 255.255.255.0
sw-multicapa(config-if)# no shutdown
sw-multicapa(config-if)# exit
```
4. Activamos enrutado en el switch
```
sw-multicapa(config)# ip routing
```
# Cosas para Routers
## Poner una IP a una interfaz
### Caso 1: Interfaz Ethernet (Difusión)
```
R1 (config)# interface GigabitEthernet 0/0
R1 (config-if)# ip address 10.0.1.1 255.255.255.0
R1 (config-if)# no shutdown
```
### Caso 2: Interfaz Serial (punto a punto)
#### Parte DCE (la del reloj)
```
R1 (config)# interface Serial 0/0/0
R1 (config-if)# ip address 10.0.2.1 255.255.255.0
R1 (config-if)# clock rate 4000000
R1 (config-if)# no shutdown
```
#### Parte DTE
```
R2(config) interface Serial 0/0/0
R1 (config-if)# ip address 10.0.2.2 255.255.255.0
R1 (config-if)# no shutdown
```
## Ver tabla de rutas
```
R1# show ip route
```
## Añadir ruta estática
```
R1(config)# ip route red máscara ip-sig-salto
> Ejemplo: R2(config)# ip route 10.0.1.0 255.255.255.0 10.0.2.1
