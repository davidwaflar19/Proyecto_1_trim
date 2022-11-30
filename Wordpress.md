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



