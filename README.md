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
### Ubuntu Desktop
Ejecutamos el comando y modificamos el archivo
```
sudo nano /etc/netplan/01-network-manager-all.yaml
```
Escribimos esto
```
network:
  version: 2
  ethernets:
       eth0:
           dhcp4: no
           addresses: [192.168.1.150/24]
           nameservers:
             addresses: [8.8.8.8, 8.8.4.4]
       eth1:
           dhcp4: yes
```
### Windows 10


### Servidor Jitsi Meet
opcional actualizar el sistema
```
sudo apt update
```
Descargamos el paquete HTTPS
```
sudo apt install apt-transport-https
```
Descargar el repositorio universe para las dependecias de  Jitsi
```
sudo apt-add-repository universe
```
Actualizamos
```
sudo apt update
```
Descargamos Apache2
```
sudo apt install apache2
```
Vamos a genera el certificado autofirmado
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/jitsifelipe.duckdns.org.key \
    -out /etc/ssl/certs/jitsifelipe.duckdns.org.crt

```
Vamos a configurar mi dominio, nesecitamos crear un archivo con el nombre de mi domio
```
sudo nano /etc/apache2/sites-available/jitsifelipe.duckdns.org
```
```
<VirtualHost *:443>
    ServerName jitsifelipe.duckdns.org
    DocumentRoot /var/www/html/local
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/jitsifelipe.duckdns.org.crt
    SSLCertificateKeyFile /etc/ssl/private/jitsifelipe.duckdns.org.key
    
    ErrorLog ${APACHE_LOG_DIR}/jitsifelipe-ssl-error.log
    CustomLog ${APACHE_LOG_DIR}/jitsifelipe-ssl-access.log combined
</VirtualHost>

```
Agregamos el repositorio de jitsi meet
```
wget -qO - https://download.jitsi.org/jitsi-key.gpg.key | sudo apt-key add -
sudo sh -c "echo 'deb https://download.jitsi.org stable/' > /etc/apt/sources.list.d/jitsi-stable.list"
sudo apt update
```
Instalamos jitsi
```
sudo apt install jitsi-meet
```
