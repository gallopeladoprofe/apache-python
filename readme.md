# Configuraciones para Python y CGI
## Para propósitos didácticos

- Crear en la carpeta de usuario `mkdir nuevohost/{app, logs}`

- Editar `apache2.conf` en `/etc/apache2/apache2.conf`, añadir:

```conf
<Directory /home/juan>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>

ServerName localhost
```

- Editar `/etc/hosts` para el `127.0.0.1 localhost`
- Crear un *Virtual Host* de nombre `nuevohost.conf` en `/etc/apache2/sites-available/nuevohost.conf`

```conf
<VirtualHost *:8080>

    ServerName localhost
    DocumentRoot /home/juan/nuevohost/app/
    ErrorLog /home/juan/nuevohost/logs/error.log
    CustomLog /home/juan/nuevohost/logs/access.log combined

    <Directory /home/juan/nuevohost/app>
        Options -Indexes +ExecCGI
        setHandler cgi-script
    </Directory>

</VirtualHost>
```

- Editar `ports.conf` en `/etc/apache2/ports.conf`, el puerto: `Listen 8080`

- Crear script en `/app/hola.py`

```Python
#!/usr/bin/env python3
print("Content-type: text/html; charset=utf-8")
print("")
print("Hola mundo")
```

- Aplicar permisos

```sh
sudo chmod +x /home/juan
sudo chmod +x /home/juan/nuevohost
sudo chmod +x /home/juan/nuevohost/app
```

- Para listar todos los módulos de Apache2 instalados en tu sistema Ubuntu, puedes usar el siguiente comando en la terminal:

`apache2ctl -M`

- Este comando muestra todos los módulos cargados y habilitados en el servidor Apache2. Si quieres listar solo los módulos disponibles pero no necesariamente habilitados, usa:

```sh
ls /etc/apache2/mods-available
```

- Y para listar los módulos habilitados, usa:

```sh
ls /etc/apache2/mods-enabled
```

- Deshabilitar módulos:

```sh
a2dismod mpm_event
a2dismod mpm_worker
a2dismod cgid
```

- Habilitar módulos:

```sh
a2enmod mpm_prefork
a2enmod cgi
```

- Habilitar del nuevo virtual host

```sh
a2ensite nuevohost.conf
```

- Verificar sitios activos

```sh
apachectl -S
```

- Gestionar el servicio de apache2

```sh
systemctl start apache2
systemctl reload apache2
systemctl stop apache2
systemctl restart apache2
```

- Verificar en el navegador `http://localhost:8080/hola.py`
