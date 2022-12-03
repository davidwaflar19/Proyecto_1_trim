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

