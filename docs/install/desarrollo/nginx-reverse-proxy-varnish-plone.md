# Configurar NGINX para servir Plone con Varnish

## Introducción

En la sección [Instalar NGINX](instalar-nginx.md), vimos cómo realizar la instalación y configuración de nuestro NGINX pero aún no está configurado para servir nuestro sitio plone `WEPPOR`.
Para ello, vamos a modificar el archivo ``/etc/nginx/sites-availables/``.

## Paso 1. Copia de seguridad

Vamos a copiar la configuración actual, por si tenemos que volver a ella. Para ello, tecleamos:

``` shell
sudo cp /etc/nginx/sites-availables/weppor.desarrollo /etc/nginx/sites-availables/weppor.desarrollo.bak
```

!!! warning "Aviso"
    Esto sobreescribirá el anterior archivo .bak que teníamos guardado. Si lo deseas, puedes dar otro nombre y así conservar ambas copias.

## Paso 2. Modificar la configuración

En el archivo ``/etc/nginx/sites-availables/weppor.desarrollo`` tenemos esta configuración:

``` nginx title="/etc/nginx/sites-availables/weppor.desarrollo"
server {
        listen 443 ssl;
        listen [::]:443 ssl;
        include snippets/self-signed.conf;
        include snippets/ssl-params.conf;

        root /var/www/weppor.desarrollo/html;
        index index.html index.htm index.nginx-debian.html;

        server_name weppor.desarrollo www.weppor.desarrollo;

        location / {
                try_files $uri $uri/ =404;
        }
}
server {
    listen 80;
    listen [::]:80;

    server_name weppor.desarrollo www.weppor.desarrollo;

    return 302 https://$server_name$request_uri;
}
```

Y lo vamos a sustituir por esto:

``` shell
sudo nano /etc/nginx/sites-availables/weppor.desarrollo
```

Añadiremos cabeceras para securizar las peticiones y haremos un **proxy-pass** al ``varnish``, donde se devolverá la petición si está cacheada o este redireccionará la petición al backend de zope para que se procese.

``` nginx title="/etc/nginx/sites-availables/weppor.desarrollo"

# This adds security headers
add_header X-Frame-Options "SAMEORIGIN";
add_header Strict-Transport-Security "max-age=15768000; includeSubDomains";
add_header X-XSS-Protection "1; mode=block";
add_header X-Content-Type-Options "nosniff";
# add_header Content-Security-Policy "default-src 'self'; img-src *; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval'";
add_header Content-Security-Policy-Report-Only "default-src 'self'; img-src *; style-src 'self' 'unsafe-inline'; script-src 'self' 'unsafe-inline' 'unsafe-eval'";

server {
        listen 443 ssl;
        listen [::]:443 ssl;
        include snippets/self-signed.conf;
        include snippets/ssl-params.conf;

        root /var/www/weppor.desarrollo/html;
        index index.html index.htm index.nginx-debian.html;

        server_name weppor.desarrollo www.weppor.desarrollo;

        location / {
                rewrite ^/(.*)$ /VirtualHostBase/https/weppor.desarrollo:443/empleado/VirtualHostRoot/$1 break;
                proxy_pass http://127.0.0.1:10050/;
                proxy_set_header        Host            $host;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        location /gestorz {
                rewrite ^/(.*)$ /VirtualHostBase/https/weppor.desarrollo:443/empleado/VirtualHostRoot/$1 break;
                proxy_pass http://127.0.0.1:10050/;
                proxy_set_header        Host            $host;
                proxy_set_header        X-Real-IP       $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        }
}
server {
    listen 80;
    listen [::]:80;

    server_name weppor.desarrollo www.weppor.desarrollo;

    return 302 https://$server_name$request_uri;
}

```
Guarda la configuración:



## Paso 3. Verificar la configuración y reiniciar NGINX

Antes de aplicar los cambios, vamos a comprobar que no tengamos errores:

``` shell
sudo nano nginx -t
```

Si todo es correcto, reiniciamos Nginx:

``` shell
sudo service nginx restart
```
``` output
nginx: [warn] "ssl_stapling" ignored, issuer certificate not found for certificate "/etc/ssl/certs/nginx-selfsigned.crt"
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## Último paso. Comprobar los cambios

Ahora, comprueba que los cambios son correctos accediendo a http://weppor.desarrollo, deberías ver tu sitio plone ``empleado`` si está creado o un error de Plone.
Hay que verificar también que varnish está operativo:

1. Abre la pestaña desarrollador de tu navegador con la tecla ++f12++.
2. Ve a la pestaña ``Red`` y después recarga la ventana ++f5++.
3. Con el ratón, pincha sobre la petición que corresponde a la página y verifica que los encabezados de la respuesta incluyen información sobre Varnish.
4. Activa y configura la caché en el panel de control, para activar el resto de funcionalides.

    ```
        URL de la solicitud: https://weppor.desarrollo/
        Método de la solicitud: GET
        Código de estado: 200 OK
        Dirección remota: 127.0.0.1:443
        Directiva del origen de referencia: strict-origin-when-cross-origin
        Age: 18
        Cache-Control: max-age=0, s-maxage=0, must-revalidate
        Connection: keep-alive
        Content-Encoding: gzip
        Content-Language: es
        Content-Type: text/html;charset=utf-8
        Date: Mon, 06 Mar 2023 10:29:01 GMT
        Expires: Fri, 08 Mar 2013 10:28:43 GMT
        grace
        Last-Modified: Mon, 06 Mar 2023 09:01:40 GMT
        Server: nginx/1.18.0 (Ubuntu)
        Transfer-Encoding: chunked
        Vary: Cookie
        Via: waitress
        {==Via: 1.1 varnish (Varnish/6.0)==}
        {==X-Cache: HIT==}
        {==X-Cache-Operation: plone.app.caching.moderateCaching==}
        {==X-Cache-Rule: plone.content.folderView==}
        {==X-Cacheable: YES==}
        X-Content-Type-Options: nosniff
        X-Frame-Options: SAMEORIGIN
        X-Frame-Options: DENY
        X-XSS-Protection: 1; mode=block
    ```

## Conclusión

Ya dispones de una instancia ZOPE, servida a través de un NGINX y Varnish. ¡Suerte! :sweat_smile:
