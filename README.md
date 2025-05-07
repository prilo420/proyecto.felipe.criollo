## proyecto.jitsi-meet.felipe.criollo
documentación
DOCUMENTACIÓN JITSI MEET

# Creamos las máquinas virtuales que nos indica la práctica en Hyper-V. 
* Ubuntu sever 
* Ubuntu Desktop
* Windows 10 o Windows 11
 

## Configuración de SHH
Dentro de la máquina de ubuntu server, introduciremos el siguiente comando para instalar las SSH en la misma.
```
sudo apt install shh*
```
## configuración de Ip estática
### Ubuntu Server

```
sudo nano /etc/netplan/00-installer-config.yaml
```
Editamos el archivp de configuracion de red

```
network:
  ethernets:
    eth0:
       dhcp4: no
       addresses: [192.168.1.100/24]
    eth1:
      dhcp4: yes
version: 2
```
Guardamos:
```
sudo netplan apply
```
