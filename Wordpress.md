###Instalación de Apache

-Priemro que nada tenemos que nada tenemos que estar en modo administrador:
```bash
$ sudo su
$ apt update
$ apt install apache2
```
-Para comprobar que se ha instalado escribimos systemctl status apache2
```bash
$ systemctl status apache2
```
![image](https://user-images.githubusercontent.com/91255833/204491638-c985816e-ed34-4269-8dba-bc197e0541d9.png)

-Ahora poenemos el cortafuegos para permitir tráfico web:
```bash
$ ufw app list
```
![image](https://user-images.githubusercontent.com/91255833/204492532-c91c91cb-637a-44d4-b4d3-bf3061868c10.png)

```bash
$ ufw app info "Apache Full"
```
![image](https://user-images.githubusercontent.com/91255833/204493361-0c33bea6-68cc-4975-ae0e-c4aaa40025eb.png)

Ahora para conocer la IP del localhost escribimos:
```bash
$ cd/etc 
$ nano hosts
```
![image](https://user-images.githubusercontent.com/91255833/204737333-a7a19de9-be37-4925-815b-f9a58aa6e79d.png)

Para ver que funciona vamos a firefox y ponemos
```bash
http://ip_localhost
```
![image](https://user-images.githubusercontent.com/91255833/204737815-07c96a04-d300-4a9d-9a66-8d4f9884a93d.png)

Ahora tenemos que crear el directorio de centro.intranet dentro del directorio /www
```bash
$ mkdir /var/www/centro.intranet
$ ls /var/www
```
![image](https://user-images.githubusercontent.com/91255833/204739021-24b2e3f7-1ae1-439b-b8d9-61fc8fea267d.png)

Y le ponemos una ip a centro.intranet por el archivo hosts como antes

![image](https://user-images.githubusercontent.com/91255833/204739599-e6d3d819-7865-4bd1-886f-66d2e070d50e.png)

##Instalar MYSQL
Ahora instalaremos en nuestro servidor MySql, para ello pondremos el siguiente comando:
```bash
$ apt update
$ apt install mysql-server
```
Y para comprobar que está instalado correctamente
```bash
systemctl status mysql
```
![image](https://user-images.githubusercontent.com/91255833/205361464-678b6fd7-492e-41ea-b161-cb077404512f.png)

##Instalación de PHP
También instalaremos PHP para ello, vamos a introducir el mismo comando:
```bash
$ apt install php libapache2-mod-php php-mysql
``` 
En la mayoría de los casos, se deseará modificar la forma mediante la cual Apache sirve archivos cuando un directorio es solicitado. Ahora, si un usuario solicita un directorio del servidor, Apache buscará, un index.html. Nosotros queremos que el servidor web le dé prioridad a los archivos PHP sobre cualquier otro archivo. Para lo cual haremos que el Apache busque el archivo index.php en primer lugar.
Para ello nos metemos en el siguiente archvo:
```bash
$ nano /etc/apache/mods-enabled/dir.conf
```
Así es su aspecto sin modificar:
![image](https://user-images.githubusercontent.com/91255833/205363995-cc1a37a3-49a3-496a-b826-7b8565578aa1.png)

Y así lo tendremos despues de modificarlo:
![image](https://user-images.githubusercontent.com/91255833/205364338-68ad2540-df35-4d69-8b4e-eb3b798fa6cc.png)

Reiniciamos apache y comprobamos su estado como hemos echo antes:
![image](https://user-images.githubusercontent.com/91255833/205365041-152211f7-6c0f-4387-9251-9b7144a781a6.png)

##Wordpresss
Ahora vamos a instalar Wordpress, para ello lo primero que tenemos que hacer es descargar los archivos de la página
![image](https://user-images.githubusercontent.com/91255833/205365669-a4a128f3-8e64-45a1-8f9e-4d62e3923751.png)

Ahora tenemos que hacer varias cosas para que funcione wordpress
-Crear un virtual host
Para ello crearemos un archvo de configuracion en la carpeta sites-abiable de apache2 con este comando:
```bash
$ nano /etc/apache2/sites-available/centro.intranet.conf
```
Dentro del fichero ponemos esto:
```apache2
<VirtualHost *:80>
    ServerName centro.intranet
    ServerAlias www.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/centro.intranet
    Errorlog ${APACHE_LOG_DIR}error.log
    Customlog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Ahora vamos a habilitar los nuevos archivos del host virtual y desactivar el predeterminado con los siguientes comandos:
```bash
$ a2ensite centro.intranet.conf
$ a2dissite 000-defult.conf
```
Despues de poner cada comando hay que reiniciar apache
![image](https://user-images.githubusercontent.com/91255833/205368795-1d0c1796-7aa5-46af-bacc-118f13b9f2bb.png)

Ahora asignamos a www.centro.intranet la misma IP que centro.intranet editando el fichero hosts
![image](https://user-images.githubusercontent.com/91255833/205369620-93a9f9e4-35d7-442d-940c-503f4fc12a92.png)

Para poder ver algo creamos un archivo básico de HTML en /var/www/centro.intranet
![image](https://user-images.githubusercontent.com/91255833/205370829-c5e1158b-775e-461e-bb43-d1e5827834cd.png)
![image](https://user-images.githubusercontent.com/91255833/205370994-7b332d22-9df5-4484-b243-05882f2d0241.png)


