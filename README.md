# Prácticas 1 y 2 - SWAP
### Por: Antonio Galdó Seiquer
Lo primero que he hecho ha sido crear 2 máquinas virtuales (con VirtualBox y Ubuntu Server 16) y configurar la red de siguiente manera:
He creado una red host-only en VirtualBox y en la configuración de cada máquina he habilitado un nuevo adaptador con esa red. Después, en las máquinas, he modificado el archivo __/etc/network/interfaces__ añadiendo lo siguiente (en la máquina SWAP1:
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.105
```
Y lo siguiente en SWAP2:
```
auto enp0s8
iface enp0s8 inet static
address 192.168.56.110
```
Tras esto hay que levantar la interfaz de red con ```ifup enp0s8``` o reiniciar y ya está.

Teniendo ya las máquinas instaladas, con el LAMP configurado y la configuración de interfaz de red para que se conectaran las máquinas entre sí y con el host, he realizado los siguientes pasos para tener una copia del servidor web en una de las máquinas de la otra.
Como ya tenemos 'rsync' instalado por defecto no hará falta instalarlo.

Lo primero que he hecho es este comando para que nuestro usuario (en este caso _xupi_) tenga permisos para interactuar con el directorio del servidor Apache:
```console
xupi@SWAP2:~$ sudo chown xupi –R /var/www
```
Tras esto he configurado el acceso a la máquina 1 con ssh sin contraseña:
```console
xupi@SWAP2:~$ ssh-keygen -b 4096 -t rsa
```
He dejado los valores por defecto para que se generen las llaves en __~/.ssh/__ y no haga falta contraseña.
A continuación he pasado la clave a la máquina 1 y con esto ya no haría falta contraseña para ssh:
```console
xupi@SWAP2:~$ ssh-copy-id 192.168.56.105
```
A continuación he configurado el crontab para que ejecute rsync cada 1 minuto con:
```console
xupi@SWAP2:~$ sudo nano /etc/crontab
```
He añadido la orden rsync `rsync -auv -e ssh 192.168.56.105:/var/www/ /var/www` y el archivo queda así:
![ ](crontab.png  "resultado de crontab:")
