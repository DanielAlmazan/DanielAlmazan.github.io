# Despliegue de Gestión Hotelera

![Apache 2 logo](apache2.png) {width=300 border-effect="none"}

![Passenger logo](passenger.png) {width=80 border-effect="none"}

En nuestro VPS con Debian 11, instalamos Apache2

```Bash
sudo apt-get install apache2
```

## Configuración básica

Lo primero, por seguridad, nos hacemos una copia del fichero `apache2.conf`,
aunque en principio no lo vamos a modificar.

```Bash
sudo cp /etc/apache2/apache2.conf /etc/apache2/apache2.conf.original
```

```Bash
sudo nano /etc/apache2/ports.conf
```

```Ini
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 8000
Listen 8080
Listen 8088
Listen 8888

<IfModule ssl_module>
	Listen 443
</IfModule>

<IfModule mod_gnutls.c>
	Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

## Instalación de Passenger

Para instalar Passenger en el servidor (Debian, en este caso), ejecutamos los siguientes comandos:

```Bash
sudo apt-get install -y dirmngr gnupg

sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 
--recv-keys 561F9B9CAC40B2F7

sudo apt-get install -y apt-transport-https ca-certificates

sudo sh -c 'echo deb https://oss-binaries.phusionpassenger.com/
apt/passenger bullseye main > /etc/apt/sources.list.d/passenger.list'

sudo apt-get update

sudo apt-get install -y libapache2-mod-passenger
```

Habilitamos el módulo de Passenger

```Bash
sudo a2enmod passenger
```

Seguidamente, reiniciamos Apache2

```Bash
sudo systemctl restart apache2
```

Vamos a comprobar que el módulo de Passenger se ha instalado correctamente

```Bash
sudo /usr/bin/passenger-config validate-install
```

```Ini
[sudo] password for {user}:
What would you like to validate?
Use <space> to select.
If the menu doesn't display correctly, press '!'

   ⬢  Passenger itself
 ‣ ⬢  Apache

-------------------------------------------------------------------------

Checking whether there are multiple Apache installations...
Only a single installation detected. This is good.

-------------------------------------------------------------------------

 * Checking whether this Passenger install is in PATH... ✓
 * Checking whether there are no other Passenger installations... ✓
 * Checking whether Apache is installed... ✓
 * Checking whether the Passenger module is correctly configured in Apache... ✓

Everything looks good. :-)
```

> Es posible que, dependiendo de la configuración de vuestra consola, no se muestre correctamente el menú de opciones. 
> En ese caso, pulsad `!` para que se muestre correctamente.

{style="note"}


Creamos el fichero de configuración de nuestro sitio web

```Bash
sudo nano /etc/apache2/sites-available/003-gestion-hotelera.conf
```

```Ini
<VirtualHost *:8080>
    ServerAdmin {email}
    ServerName amorenoiborra.es
    DocumentRoot "/home/despliegue/Gestion-Hotelera"
    PassengerAppRoot "/home/despliegue/Gestion-Hotelera"
    PassengerAppType node
    PassengerStartupFile index.js
    <Directory "/home/despliegue/Gestion-Hotelera">
        Allow from all
        Options -MultiViews
        Require all granted
    </Directory>
</VirtualHost>
```

Indicamos que escuche en el puerto 8080, que es el que hemos configurado en `ports.conf`,
que el directorio raíz de nuestro sitio web es `/home/despliegue/Gestion-Hotelera`,
que el tipo de aplicación es `node`, que el fichero de inicio es `index.js` y que el
directorio `/home/despliegue/Gestion-Hotelera` tiene permisos de lectura para todos los usuarios.


## Habilitar el sitio

Creamos un enlace simbólico en `sites-enabled` para habilitar el sitio

```Bash
sudo ln -s /etc/apache2/sites-available/003-gestion-hotelera.conf \
/etc/apache2/sites-enabled/003-gestion-hotelera.conf
```

En caso de querer deshabilitar el sitio, bastaría con borrar el enlace simbólico.


## Recargar Apache2

```Bash
sudo systemctl reload apache2
```

## Instalación de NodeJS

Ahora vamos a instalar NodeJS en nuestro servidor Debian 11

```Bash
sudo apt-get install curl
curl -sL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Comprobamos la versión de NodeJS y de NPM

```Bash
node -v
```
```Ini
v20.0.0
```
```Bash
npm -v
```
```Ini
10.3.0
```
