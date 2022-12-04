### Instalacion de nginx
Para instalar nginx escribimos el siguiente comndo:
```bash
$ sudo apt update 
$ sudo apt install nginx
```
Anets de empezar con nginx tenemos que configurar el firewall de forma que permita el acceso al servicio
 ```bash
 $ sudo ufw app list
 ``` 
 Nos muestra una lista de aplicaciones:
 ![image](https://user-images.githubusercontent.com/91255833/205452632-ff2d5834-390b-47ad-9199-574cc191afa3.png)

Es recomendable activar el perfil mas retrictivo:
```bash
 $ sudo ufw allow 'Nginx HTTP'
 ``` 
 ![image](https://user-images.githubusercontent.com/91255833/205452729-cb4d227f-60ef-48a7-a630-ce2c89217900.png)

 Para verficar el cambio:
 ```bash
 $ sudo ufw status
 ```
 En caso de estar inactivo:
 ```bash
 $ sudo ufw enable
 ```
 Y lo comprobamos otra vez:
 ![image](https://user-images.githubusercontent.com/91255833/205452876-122ab3ce-2d56-4af2-814f-21a458dc918a.png)

## Verificar el servidor web
Para ver si está activo escribimos:
 ```bash
 $ systemctl status nginx
 ```
 ![image](https://user-images.githubusercontent.com/91255833/205453006-6cec9869-f6ab-4042-89b3-191cd23030ae.png)

Aqui accedemos a la página predeterminada de nginx:
![image](https://user-images.githubusercontent.com/91255763/205108711-079ebf61-b567-4d74-8560-473dfe16a371.png)

## Ficheros de configuración y bloques de nginx
Para que el servidor funcione debemos crear un fichero de configuración de nginx, yo modificaré el archivo por defecto de la forma siguiente:
![image](https://user-images.githubusercontent.com/91255833/205453538-dda29907-a000-4e44-9e0b-61997c2c1a98.png)

Ahora editamos el archvivo hosts y le ponemos una ip.
Tambien creamos un directorio de la siguiente manera:
```bash
$ sudo mkdir -p /var/www/servidor2.centro.intranet/html
```
Y lo vamos a restringir a $USER
```bash
$ sudo chown -R $USER:$USER /var/www/servidor2.centro.intranet/html
```
Tambien cambiamos los permisos:
```bash
$ sudo chmod -R 755 /var/www/servidor2.centro.intranet/html
```
Ahora creo un index.html para ver algo:
```html
<html>
    <head>
        <title>Nginx funciona(O eso creo)</title>
    </head>
    <body>
        <h1>Si lees esto es que me tienes que poner mas nota =)</h1>
    </body>
</html>
```
## Configuracion de puerto de acceso y bloques de nginx
Para hacer que nginx reciba por el puerto 8080 debemos modificar el fichero de configuración de nginx con el siguiente comando:
```bash
$ sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```
Modificamos listen 80 y listen [::]:80 default_server por listen 8080 y listen [::]:8080 default_server:

![image](https://user-images.githubusercontent.com/91255763/205137228-5191d86f-d234-47fa-bdd0-6d17a4bdf0f4.png)

Despues de configurar el puerto debemos habilitar el archivo creando un enlace desde el mismo al directorio sites-enabled (habilitado para sitios), el cual Nginx usa para leer durante el inicio:

```bash
$ sudo ln -s sudo ln -s /etc/nginx/sites-available/servidor2.centro.intranet /etc/nginx/sites-enabled/
```
## Instalar mysql
para isntalar mysql
```bash
$ sudo apt install mysql-server
```
Debemos cambiar la contraseña de root 
```bash
$ sudo mysql
```
```sql
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
La password corresponde a una contraseña alfanumerica, si ejecutamos el comando FLUSH PRIVILEGES, el servidor vuelve a cargar la tabla de permisos y aplica los nuevos cambios:
```sql
mysql> FLUSH PRIVILEGES;
```
![image](https://user-images.githubusercontent.com/91255763/205155740-3156189d-8533-4508-9150-a9ce6cdba526.png)

Podemos ver en este ejemplo que el usuario root de MYSQL ahora se autentica utilizando una contraseña. una vez que confirmemos esto, podemos salir del shell de mysql.
-Importante: Despues de configurar root ya no podremos utilizar sudo mysql. En su lugar utilizaremos lo siguiente:
```bash
$ mysql  -u root -p
```
Una vez que pongamos la contraseña, podremos ver la línea de comandos de mysql.
Al instalar Mysql La mayoría de los estándares consideran que las configuraciones predeterminadas nuevas son débiles y causan preocupación por permitir potencialmente la intrusión o la explotación de piratas informáticos. Una solución es ejecutar el script de seguridad de instalación.
Primero, usamos el siguiente comando para iniciar el (mysql_secure_installation):
```bash
$ sudo mysql_secure_installation
```
![image](https://user-images.githubusercontent.com/91255833/205503558-876d90d4-2429-44cb-96f3-4845af0b2a55.png)

## Instalar PHP
Dado que Nginx no contiene procesamiento de PHP nativo como otros servidores web, tenemos que instalar php-fpm, que significa “administrador de procesos de fastCGI”. Indicaremos a Nginx que transmita solicitudes de PHP a este software para su procesamiento.
```bash
$ sudo add-apt-repository universe
```
Instalamos el módulo php-fpm junto con un paquete auxiliar adicional, php-mysql, que permitirá a PHP comunicarse con el backend de su base de datos.
```bash
$ sudo apt install php-fpm php-mysql
```
Con esto, habremos instalado todos los componentes requeridos de la pila LEMP, pero aún tenemos que realizar algunos cambios en la configuración para indicar a Nginx que utilice el procesador PHP para contenido dinámico.
Para hacerlo, abrimos un nuevo archivo de configuración de bloques de servidor dentro del directorio /etc/nginx/sites-available/.
```bash
$ sudo nano /etc/nginx/sites-available/servidor2.centro.intranet
```
Agregamos el siguiente contenido, tomado y modificado ligeramente a partir del archivo de configuración predeterminado de bloques de servidor, a su nuevo archivo de configuración de bloques de servidor otra vez:
```nginx
server {
        listen 80;
        root /var/www/servidor2.centro.intranet/html;
        index index.html index.htm index.nginx-debian.html;
        server_name servidor2.centro.intranet www.servidor2.centro.intranet;
        location / {
                try_files $uri $uri/ =404;
        }
        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }
}
```
## Crear un archivo PHP para comprobar
Nuestra pila LEMP ahora debería estar configurada por completo. Podemos probarla para validar la ejecución correcta de archivos .php a partir del procesador PHP por parte de Nginx.
Para ello creamos un archivo php que se llama info.php:
```bash
$ sudo nano /var/www/servidor2.centro.intranet/html/info.php
```
Intoducimos lo siguiente que hará que el servidor muestre información:
```php
<?php
phpinfo();
```
Ahora en un navegador buscamos la ip de nuestro servidor terminado de /info.php y nos saldrá algo parecido a esto:
![image](https://user-images.githubusercontent.com/91255833/205504201-1ff0c109-c9b0-4dd1-a069-bc916c3b2115.png)

## Instalación de PhpMyadmin
De manera predeterminada, el repositorio de Ubuntu viene con phpMyAdmin y las dependencias requeridas. Sin embargo, como ocurre a menudo con las versiones de Ubuntu LTS, la versión y la compilación están muy atrasadas.
Para ello instalaremos la última versión:
```bash
DATA="$(wget https://www.phpmyadmin.net/home_page/version.txt -q -O-)"
URL="$(echo $DATA | cut -d ' ' -f 3)"
VERSION="$(echo $DATA | cut -d ' ' -f 1)"
wget https://files.phpmyadmin.net/phpMyAdmin/${VERSION}/phpMyAdmin-${VERSION}-all-languages.tar.gz
https://es.linuxcapable.com/how-to-install-phpmyadmin-with-lemp-on-ubuntu-22-04-lts/#Install_phpMyAdmin
```
Ahora extraemos el archivo:
```bash
$ tar xvf phpmyadmin-${VERSION}-all-languages.tar.gz
```
## Configurar PhpMyadmin
El primer paso es mover todos los archivos a /var/www, hemos utilizado el comando mv:
```bash
$ sudo mv phpMyAdmin/ /var/www/phpmyadmin 
```
Por defecto PhpMyadmin no tiene un archivo tmp, así que lo tenemos que crear:
```bash
$ sudo mkdir -p /var/www/phpmyadmin/tmp
```
En el directorio phpMyAdmin, se incluye un archivo de ejemplo de configuración predeterminado. Tenemos que cambiar el nombre de este archivo para que phpMyAdmin reconozca la configuración.
Sin embargo, para la copia de seguridad, utilizará el comando CP para crear una copia y mantener el valor predeterminado como copia de seguridad si se comete algún error en la ubicación /var/www/phpmyadmin/ .
Copiamos config.muestra.inc.php a config.inc.php con el siguiente comando:
```bash
$ sudo cp /var/www/phpmyadmin/config.sample.inc.php /var/www/phpmyadmin/config.inc.php
```
Despues abrimos el documento, phpMyAdmin usa un Cifrado de pez globo. Nos vamos hasta la línea que comienza con:
```php
$ cfg ['blowfish_secret'].
```
Las líneas se verán como:
```php
$cfg['blowfish_secret'] = ''; /* YOU MUST FILL IN THIS FOR COOKIE AUTH! */
```
Tenemos que asignar una cadena de 32 caracteres aleatorios entre las comillas simples. La forma más sencilla de lograrlo es utilizando el programa pwgen. Instalamos pwgen con el siguiente comando:
```bash
$ sudo apt install pwgen -y
```








