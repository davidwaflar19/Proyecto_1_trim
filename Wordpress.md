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

