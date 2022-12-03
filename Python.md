###Instalar python

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

Ahora creamos un directorio para departamentos.centro.intranet, que la creamos en /var/www/html
![image](https://user-images.githubusercontent.com/91255833/205438766-37f2332e-a6d0-4653-a5e7-bc9fdf42b4e1.png)

Le asignamos una IP a departamentos.centros.intranet y a www.departamentos.centros.intranet a través del archivo hosts como hemos visto antes:
![image](https://user-images.githubusercontent.com/91255833/205438890-f16efa57-5c98-4b83-8ac3-c88e6ce1bbe2.png)

Para que podamos ver el sitio, creamos un host virtual en el fichero sites-available de /etc/apache2
```bash 
$ nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```
```apache2
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    ServerAlias www.departamentos.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/departamentos.centro.intranet
    Errorlog ${APACHE_LOG_DIR}error.log
    Customlog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Damos de alta el sitio con:
```bash
$ a2ensite departamentos.centro.intranet
```
Para comprobar que existe buscamos en el navegador:
![image](https://user-images.githubusercontent.com/91255833/205439401-13ebfdab-4007-4ee3-9309-cf94c6a5ed11.png)

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

##Python
PAra poder user python instalaremos el siguiente modificador:
```bash
$ apt-get install libapache2-mod-wsgi
```
Ahora en departamentos.centro.intranet cremaos 2 directorios mypythonapp y publi_html, uno alamacena python y otro la sirve
```bash
$ mkdir /var/www/html/departementos.centro.intranet/mypythonapp
$ mkdir /var/www/html/departementos.centro.intranet/public_html
``` 
![image](https://user-images.githubusercontent.com/91255833/205439809-d0914b16-eb94-453f-886a-4215a5ed2e51.png)

Crearemos un directorio destinado a almacenar logs de errores y accesos a la Web App:
```bash
$ mkdir /var/www/html/departementos.centro.intranet/logs
```
Creamos un controlador para la aplicación ya que todas las peticiones realizadas por el usuario, serán manejadas por un único archivo, que estará almacenado en nuestro directorio mypythonapp
```bash
$ echo '# -*- coding: utf-8 -*-' > mypythonapp/controller.py
``` 
Ahora modificamos el virtual host y quedará tal que así:
```bash
<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    ServerAlias www.departamentos.centro.intranet
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html/departamentos.centro.intranet
    WSGIScriptAlias /var/www/html/departamentos.centro.intranet/controller.py 
    Errorlog /var/www/html/departamentos.centro.intranet/logs/error.log
    Customlog /var/www/html/departamentos.centro.intranet/logs/access.log combined
     <Directory /> 
    Options FollowSymLinks 
    AllowOverride All 
  </Directory> 
</Vi
![image](https://user-images.githubusercontent.com/91255763/204606251-0f9d95d3-09b6-4266-a823-55e4ca8520b7.png)

Una página web que me ha ayudado a hacer esto:
https://uniwebsidad.com/libros/python/capitulo-13/python-bajo-apache

