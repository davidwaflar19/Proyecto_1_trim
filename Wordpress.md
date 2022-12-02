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
En la mayoría de los casos, desearás modificar la forma mediante la cual Apache sirve archivos cuando un directorio es solicitado. En este momento, si un usuario solicita un directorio del servidor, Apache buscará, en primera instancia, un archivo llamado index.html. Nosotros queremos que el servidor web le dé prelación a los archivos PHP sobre cualquier otro archivo. Para lo cual haremos que el Apache busque el archivo index.php en primer lugar.

```bash
$ nano /etc/apache/mods-enabled/dir.conf
```
Su aspecto inicial es el siguiente

![image](https://user-images.githubusercontent.com/91255763/204391170-790abf86-a1fc-4318-b84d-3d1b221c762d.png)

Pero debemos modificarlo para que tenga el siguiente

![image](https://user-images.githubusercontent.com/91255763/204391339-6a2b0756-77a2-4567-98bb-d7705f6f7ce8.png)

Y debemos reiniciar apache

```bash
$ systemctl restart apache2
```
y debemos comprobar el estado de apache2 con:

```bash
$ systemctl status apache2
```
![image](https://user-images.githubusercontent.com/91255763/204391812-fddd3e6a-3b92-4926-a9cb-6b2837abcf05.png)

Con esto ya esta instalado php.
## Wordpress
Para instalar wordpress primero debemos decargar los archivos de wordpres desde la pagina oficial

![image](https://user-images.githubusercontent.com/91255763/204392942-3a4c5bcf-727d-4c6d-ad2d-03e9cb4d6fdd.png)

Ahora debemos hacer ciertas configuraciones en apache2 para poder hacer un sitio funcional con wordpress.
### Crear un virtual host
Para esto debemos crear un archivo de configuración de apache2 dentro de la carpeta sites-available de apache2 con el siguiente comando:

```bash
$ mkdir /etc/apache2/sites-available/centro.intranet.conf
```
Debemos editarlo y para ello 

```bash
$ nano /etc/apache2/sites-available/centro.intranet.conf
```
Dentro de este fichero debemos escribir lo siguiente

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

Ahora debemos habilitar los nuevos archivos de host virtual y deshabilitar el prederminado con el siguiente comando:

```bash
$ a2ensite centro.intranet.conf
$ a2endissite 000-defult.conf
```
![image](https://user-images.githubusercontent.com/91255763/204397945-50a09794-6d37-4e3e-8ac8-fb548e0a425f.png)


Ahora debemos reiniciar apache2
```bash
$ systemctl restart apache2
$ systemctl status apache2
```
Ahora debemos asignar a www.centro.intranet la misma ip que a centro.intranet en el fichero hosts de /etc

![image](https://user-images.githubusercontent.com/91255763/204397729-e36ed0f2-e489-425d-8326-5c4006c49e18.png)

Para comprobar que funciona podemos crear un fichero index.html en /var/www/centro.intranet:

```bash
$ nano /var/www/centro.intranet
````

![image](https://user-images.githubusercontent.com/91255763/204467462-aba78677-4d50-4dd4-951b-667dd1204e5f.png)


![image](https://user-images.githubusercontent.com/91255763/204467564-2fe36eaf-53f6-4b3c-be28-1ec067c15d2a.png)


Con las preparaciones hechas solo nos queda instalar wordpress.
Ya tenemos el .zip de wordpress decargado y debemos descomprimirlo y mover el directorio resultante wordpress/ a la carpeta /var/www/centro.intranet:

```bash
$ unzip wordpress-6.1.1-es_ES.zip
$ mv wordpress/ /var/www/centro.intranet
$ cd /var/www/centro.intranet
$ ls 
```
![image](https://user-images.githubusercontent.com/91255763/204488515-80c86906-cdb5-41fd-9847-a24d522d8e36.png)

Después de moverlo debemos extraer el conrtenido de la carpeta  wordpress/ 

```bash
$ mv wordpress/* .
$ ls
```

![image](https://user-images.githubusercontent.com/91255763/204488918-7d9efd38-2725-4f72-9b60-5cf321b3a8eb.png)

Y debemos borrar el directorio wordpress/

```bash
$ rm -rf wordpress
$ ls
``` 
Ahora debemos cambiar los permisos de la carpeta centro.intranet para esto:

```bash 
$ cd ..
$ chown -R www-data:www-data centro.intranet
$ ls -l
```
![image](https://user-images.githubusercontent.com/91255763/204490531-74ac3816-9020-426f-a405-d2adce2f7062.png)

Como podemos ver los permisos a la carpeta han cambiado.
Ahora debemos crear una base de datos en mysql para wordpress, agrtegar un usuario y garantizarle todos los privilegios

```bash
$ mysql -u root -p
```
```sql
$ create database worpdress;
```
```sql
$ create user 'wordpressuser'@'localhost' identified by 'wordpressuser';
$ grant all privileges on wordpress*. to 'wordpressuser'@'localhost';
```
Con esto ya tendriamos la base de datos y el usuario para wordpress.
Buscamos en el navegador www.centro.intranet/ y debera aparecernos la pagina de wordpress

![image](https://user-images.githubusercontent.com/91255763/204495482-abc97c5f-f86b-4d29-8085-dee14c9ff6a0.png)

Solo nos queda introoducir las credenciales del usuario que hemos creado

![image](https://user-images.githubusercontent.com/91255763/204496126-ddf96b36-ac84-4dd0-b3ab-5a7080926808.png)

![image](https://user-images.githubusercontent.com/91255763/204496869-91de3552-0387-4e21-8f0c-5e6d69a63bcd.png)
![image](https://user-images.githubusercontent.com/91255763/204496944-90338a2f-42ce-4998-bbf3-7db739ea8492.png)
![image](https://user-images.githubusercontent.com/91255763/204497058-ff8714e8-4ee5-407f-8fb6-0705e2314dd4.png)










