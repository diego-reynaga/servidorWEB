# INSTALACION Y CONFIGURACION DE UN SERVIDOR WEB EN FEDORA

### 1. Actualizamos los repositorios.
```bash
sudo dnf update -y
```

### 2. Instalamos nano
```bash
sudo dnf install wget curl nano -y
```

### 3. Instalamos apache
```bash
sudo dnf install httpd -y
```

### 4. habilitar y iniciar el servicio
```bash
sudo systemctl enable httpd
```
```bash
sudo systemctl start httpd
```

### 5. Verificar el estado del servicio
```bash
sudo systemctl status httpd
```

### 6. configuar el firewall para permitir trafico HTTP(80) y HTTPS(443)
```bash
sudo firewall-cmd --permanent --add-service=http
```
```bash
sudo firewall-cmd --permanent --add-service=https
```
---------------------------------------------------------
```bash
sudo firewall-cmd –reload
```

### 7. Probar el servidor web
```bash
sudo dnf install firefox
```
> [!NOTE]
>INSTALAMOS FIREFOX SOLO SI NO LO TENEMOS INSTALADO.
-------------------------------------------------------------- 
>[!NOTE]
>Abre un navegador web y accede a la dirección IP del servidor. Deberías ver la página predeterminada de Apache.

-----------------------------------------------
## INSTALACION DE MARIADB
-----------------------------------------
### 8. Instalacion de mariadb
```bash
sudo dnf install mariadb-server -y
```

### 9. habilitar y iniciar el servicio
```bash
sudo systemctl enable mariadb
```
```bash
sudo systemctl start mariadb
```

### 10. configurar la seguridad de mariadb
>[!NOTE]
>Ejecuta el script de configuración de seguridad para establecer una contraseña de root y eliminar configuraciones inseguras.

```bash
sudo mysql_secure_installation
```
#### Sigue las indicaciones:
1. Establece una contraseña de root.
2. Elimina usuarios anónimos.
3. Prohíbe el inicio de sesión remoto para root.
4. Elimina la base de datos de prueba.
5. Recarga los privilegios.

### 11. probar la conexion a mysql
```bash
mysql -u root -p
```

## CONFIGURACION BASICA DE APACHE
--------------------------------------------------
### 12. Crear un archivo de configuracion virtual host
```bash
sudo mkdir -p /var/www/html/paginadeprueba.com
```
### 13. establecer los permisos adecuados
```bash
sudo chown -R apache:apache /var/www/html/paginadeprueba.com
```
```bash
sudo chmod -R 755 /var/www/html/paginadeprueba.com
```
### 14. crear un archivo de configuracion para el sitio
```bash
sudo nano /etc/httpd/conf.d/paginadeprueba.com.conf
```
```bash
<VirtualHost *:80>
    ServerAdmin admin@paginadeprueba.com
    ServerName paginadeprueba.com
    ServerAlias www.paginadeprueba.com
    DocumentRoot /var/www/html/paginadeprueba.com
    ErrorLog /var/log/httpd/paginadeprueba.com-error.log
    CustomLog /var/log/httpd/paginadeprueba.com-access.log combined
</VirtualHost>
```
------------------------------------------------------

### 15. reiniciamos apache
```bash
sudo systemctl restart httpd
```

### 16. creamos un archivo index.html
```bash
sudo nano /var/www/html/paginadeprueba.com/index.html
```
------------------------------------------------
```bash
<html>
<head>
</head>
<body>
<h1>HOLA MUNDO</h1>
</body>
</html>
```

## CONEXION APACHE-MYSQL
---------------------------------------------------------
### 17. instalar php y el modulo de conexion con MySQL
```bash
sudo dnf install php php-mysqlnd -y
```

### 18. reiniciar apache
```bash
sudo systemctl restart httpd
```

### 19. crear un archivo php de prueba
```bash
sudo nano /var/www/html/paginadeprueba.com/info.php
```
```bash
<?php
echo "HOLA MUNDO";
phpinfo();
?>
```
-----------------------------------------------------------------
>[!NOTE]
>Accede a http://localhost/info.php desde un navegador para verificar la configuración de PHP.

```bash
http://localhost/info.php
```







