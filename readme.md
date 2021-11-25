# README.md
>Acortador de URL basado en PHP, de código abierto, llamado YOURLS.

![login](https://user-images.githubusercontent.com/87372312/143271359-28f97f57-0577-487e-9394-6a464e760053.png)


## YOURLS Features
>Software gratuito y de código abierto,
>
>Privado (solo sus enlaces) o Público (todo el mundo puede crear enlaces cortos, está bien para una intranet),Excelente arquitectura de complementos y docenas de complementos para implementar fácilmente nuevas funciones,Prácticos marcadores para acortar y compartir enlaces fácilmente.
>
>Estadísticas impresionantes : informes históricos de clics, seguimiento de referencias, ubicación geográfica de visitantes.
>
>API de desarrollador para integrar YOURLS en otras aplicaciones,
>Instalador amigable ,¡Archivos de muestra para crear su propia interfaz pública y más!

## Estadísticas para cada URL corta

![stats](https://user-images.githubusercontent.com/87372312/143271200-373be6d5-9fa3-4372-8422-ac3b61f1b23b.gif)




## Datos Tecnicos

- YOURLS version: 1.8.2
- YOURLS DB Ver: 506
- PHP version: 7.4.3
- Server OS Build: Linux
- Server & version: Apache/2.4.41 (Ubuntu)
- Browser information: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36


## Instalar el Código
Vamos a crear un directorio, descarga y desempaca el código:
```bash
mkdir /var/www/yourls
cd /var/www/yourls
git clone https://github.com/YOURLS/YOURLS/archive/1.7.tar.gz
tar -zxvf 1.7.tar.gz 
mv YOURLS-1.7/ yourls
```
Crea un archivo de configuración de sitio en Apache:

```bash
sudo nano /etc/apache2/sites-available/yourls.conf
```
Pega y personaliza la siguiente configuración del sitio:

```bash
<VirtualHost *:80>
   ServerName your-yourls-domain.com
   DocumentRoot /var/www/yourls
   DirectoryIndex index.php
   <Directory /var/www/yourls/>
      AllowOverride All
      Order Deny,Allow
      Allow from all
   </Directory>
</VirtualHost>
```

Habilita el sitio y reinicia Apache:

```bash
sudo a2ensite yourls.conf 
sudo service apache2 reload
```

Creemos una base de datos MySQL para el YOURLS a utilizar:

```bash
mysql -uroot -p

create database yourls;
grant all privileges on yourls.* TO "yourls_db_user"@"localhost" identified by "yourls-pwd";
flush privileges;
exit;
```



## Configura el Sitio YOURLS

Ahora que nuestro sitio en Apache y nuestra base de datos MySQL está disponibles, configuremos el código un poco más.
Comienza con copiar el ejemplo de configuración para un archivo y temporalmente permite escribir permisos para la instalación.

```bash
cd /var/www/yourls
cp ./user/config-sample.php ./user/config.php
chmod 0666  ./user/config.php
vim ./user/config.php
```

Primero, configura la configuración de tu base de datos basada en cómo configuraste MySQL arriba. Puedes seguir los ajustes en la configuración en la documentación del sitio de YOURLS:

```bash
/*
 ** MySQL settings - You can get this info from your web host
 */
 
/** MySQL database username */
define( 'YOURLS_DB_USER', 'your db user name' );
 
/** MySQL database password */
define( 'YOURLS_DB_PASS', 'your db password' );
 
/** The name of the database for YOURLS */
define( 'YOURLS_DB_NAME', 'yourls' );
 
/** MySQL hostname.
 ** If using a non standard port, specify it like 'hostname:port', eg. 'localhost:9999' or '127.0.0.1:666' */
define( 'YOURLS_DB_HOST', 'localhost' );
 
/** MySQL tables prefix */
define( 'YOURLS_DB_PREFIX', 'yourls_' );
```

Luego, proporciona el nombre de tu sitio de dominio elegido (la URL) y la contraseña inicial de usuario. Ingesar las contraseñas en texto plano aquí está bien temporalmente, porque YOURLS las encriptará.

```bash
/** YOURLS installation URL -- all lowercase and with no trailing slash.
 ** If you define it to "http://site.com", don't use "http://www.site.com" in your browser (and vice-versa) */
define( 'YOURLS_SITE', 'http://site.com' );
 
/** Username(s) and password(s) allowed to access the site. Passwords either in plain text or as encrypted hashes
 ** YOURLS will auto encrypt plain text passwords in this file
 ** Read http://yourls.org/userpassword for more information */
$yourls_user_passwords = array(
    'username' => 'password',
  'username2' => 'password2' // You can have one or more 'login'=>'password' lines
    );
```

También necesitamos crear un archivo .htacces y asegurarnos que el Apache mod_rewrite esté activo:

```bash
sudo a2enmod rewrite
sudo nano /var/www/yourls/.htaccess
```

Pega el archivo predeterminado .htaccess en:

```bash
  # BEGIN YOURLS
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^.*$ /yourls-loader.php [L]
</IfModule>
 # END YOURLS
```






### Notes
If you want to learn all about markdown i recommend you visit the site [markdown.es](https://markdown.es/sintaxis-markdown/)
