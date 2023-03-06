## Introducción
TLS , o seguridad de la capa de transporte, y su predecesor SSL , que significa capa de conexión segura, son protocolos web que se utilizan para proteger y cifrar el tráfico en una red informática.

Con TLS/SSL, los servidores pueden enviar tráfico de forma segura entre el servidor y los clientes sin la posibilidad de que terceros intercepten los mensajes. El sistema de certificados también ayuda a los usuarios a verificar la identidad de los sitios con los que se conectan.

En esta guía, le mostraremos cómo configurar un certificado SSL autofirmado para usar con un servidor web Nginx en un servidor Ubuntu 20.04.

!!! note "Nota"
    un certificado autofirmado cifrará la comunicación entre su servidor y cualquier cliente. Sin embargo, debido a que no está firmado por ninguna de las autoridades de certificación de confianza incluidas con los navegadores web, los usuarios no pueden usar el certificado para validar la identidad de su servidor automáticamente.

    Un certificado autofirmado puede ser apropiado si no tiene un nombre de dominio asociado con su servidor y para instancias donde la interfaz web encriptada no está orientada al usuario. Si tiene un nombre de dominio, en muchos casos es mejor usar un certificado firmado por una CA. Puede averiguar cómo configurar un certificado de confianza gratuito con el proyecto [Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04) .

## Requisitos previos
Necesitará tener instalado el servidor web Nginx. Siga nuestra guía [Instalar NGINX y configurar las reglas del firewall](instalar-nginx.md). Asegúrese de completar el Paso 5 de ese tutorial y configurar un bloque de servidor, ya que esto será necesario para probar si Nginx puede cifrar las conexiones usando su certificado autofirmado.

### Paso 1: Crear el certificado SSL
TLS/SSL funciona mediante una combinación de un certificado público y una clave privada. La clave SSL se mantiene en secreto en el servidor y cifra el contenido enviado a los clientes. El certificado SSL se comparte públicamente con cualquiera que solicite el contenido. Se puede utilizar para descifrar el contenido firmado por la clave SSL asociada.

Puede crear un par de clave y certificado autofirmados con OpenSSL en un solo comando:

``` shell
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt
```

Aquí hay un desglose de lo que hace cada parte de este comando:

- ``sudo``: El sudocomando permite a los miembros del sudogrupo elevar temporalmente sus privilegios a los de otro usuario (el superusuario o usuario raíz , de forma predeterminada). Esto es necesario en este caso, ya que estamos creando el certificado y el par de claves en el directorio ``/etc/``, al que solo puede acceder el usuario raíz u otras cuentas privilegiadas.
- ``openssl``: esta es la herramienta de línea de comando básica para crear y administrar certificados, claves y otros archivos de OpenSSL.
- ``req``: este subcomando especifica que queremos usar la administración de solicitud de firma de certificado (CSR) X.509. El "X.509" es un estándar de infraestructura de clave pública al que se adhieren SSL y TLS para su gestión de claves y certificados. Queremos crear un nuevo certificado X.509, por lo que estamos usando este subcomando.
- ``-x509``: Esto modifica aún más el subcomando anterior al decirle a la utilidad que queremos hacer un certificado autofirmado en lugar de generar una solicitud de firma de certificado, como sucedería normalmente.
- ``-nodes``: Esto le dice a OpenSSL que omita la opción de proteger nuestro certificado con una frase de contraseña. Necesitamos Nginx para poder leer el archivo, sin la intervención del usuario, cuando se inicia el servidor. Una frase de contraseña evitaría que esto suceda porque tendríamos que ingresarla después de cada reinicio.
- ``-days 365``: Esta opción establece el período de tiempo durante el cual el certificado se considerará válido. Lo configuramos para un año aquí.
- ``-newkey rsa:2048``: Esto especifica que queremos generar un nuevo certificado y una nueva clave al mismo tiempo. No creamos la clave que se requiere para firmar el certificado en un paso anterior, por lo que debemos crearla junto con el certificado. La parte ``rsa:2048`` le dice que cree una clave RSA de 2048 bits de longitud.
- ``-keyout``: Esta línea le dice a OpenSSL dónde colocar el archivo de clave privada generado que estamos creando.
- ``-out``: Esto le dice a OpenSSL dónde colocar el certificado que estamos creando.

Como se indicó anteriormente, estas opciones crearán un archivo de clave y un certificado. Después de ejecutar este comando, se le harán algunas preguntas sobre su servidor para incrustar la información correctamente en el certificado.

!!! warning "Importante"
    Llene las indicaciones apropiadamente. **La línea más importante es la que solicita el Common Name (e.g. server FQDN or YOUR name). Debe ingresar el nombre de dominio asociado con su servidor o, más probablemente, la dirección IP pública de su servidor**.

La totalidad de las indicaciones tendrá el siguiente aspecto:

``` Output
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:New York City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Bouncy Castles, Inc.
Organizational Unit Name (eg, section) []:Ministry of Water Slides
Common Name (e.g. server FQDN or YOUR name) []:server_IP_address
Email Address []:admin@weppor.desarrollo
```

Los dos archivos que creó se colocarán en los subdirectorios correspondientes del directorio ``/etc/ssl``.

Al usar OpenSSL, también debe crear un grupo fuerte de Diffie-Hellman (DH), que se usa para negociar [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) con los clientes.

Puedes hacer esto escribiendo:

``` shell
sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096
```

Esto tomará un tiempo, pero cuando termine, tendrá un grupo DH fuerte ``/etc/nginx/dhparam.pemque`` se usará durante la configuración.

###  Paso 2: Configurar Nginx para usar SSL

Ahora que se han creado sus archivos de clave y certificado en el directorio ``/etc/ssl``, deberá modificar su configuración de Nginx para aprovecharlos.

En primer lugar, creará un fragmento de código de configuración con la información sobre la clave SSL y las ubicaciones de los archivos del certificado. Luego, creará un fragmento de configuración con una configuración SSL fuerte que se puede usar con cualquier certificado en el futuro. Finalmente, ajustará los bloques de su servidor Nginx utilizando los dos fragmentos de configuración que ha creado para que las solicitudes SSL puedan manejarse adecuadamente.

Este método de configuración de Nginx le permitirá mantener limpios los bloques del servidor y colocar segmentos de configuración comunes en módulos reutilizables.

#### Creación de un fragmento de código de configuración que apunte a la clave y el certificado SSL
Primero, use su editor de texto preferido para crear un nuevo fragmento de configuración de Nginx en el /etc/nginx/snippetsdirectorio. El siguiente ejemplo utiliza nano.

Para distinguir adecuadamente el propósito de este archivo, asígnele el nombre self-signed.conf:
``` shell
sudo nano /etc/nginx/snippets/self-signed.conf
``` 

Dentro de este archivo, debe establecer la directiva ``ssl_certificate`` en su archivo de certificado y la clave ``ssl_certificate_key`` asociada. Esto se verá como lo siguiente:

``` cfg title="/etc/nginx/snippets/self-signed.conf"
ssl_certificate /etc/ssl/certs/nginx-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/nginx-selfsigned.key;
```

Cuando haya agregado esas líneas, guarde el archivo y salga del editor.

#### Creación de un fragmento de configuración con configuración de cifrado fuerte

A continuación, creará otro fragmento que definirá algunas configuraciones de SSL. Esto configurará Nginx con un sólido conjunto de cifrado SSL y habilitará algunas características avanzadas que ayudarán a mantener su servidor seguro.

Los parámetros que establezca se pueden reutilizar en futuras configuraciones de Nginx, por lo que puede darle al archivo un nombre genérico:
``` shell
sudo nano /etc/nginx/snippets/ssl-params.conf
```

Para configurar Nginx SSL de forma segura, adaptaremos las recomendaciones de [Cipherlist.eu](https://cipherlist.eu/) . [Cipherlist.eu](https://cipherlist.eu/) es un recurso útil y digerible para comprender la configuración de cifrado utilizada para el software popular.

!!! note "Nota"
    estas configuraciones sugeridas de Cipherlist.eu ofrecen una seguridad sólida. A veces, esto tiene el costo de una mayor compatibilidad con el cliente. Si necesita admitir clientes más antiguos, hay una lista alternativa a la que se puede acceder haciendo clic en el enlace de la página con la etiqueta "Sí, denme un conjunto de cifrado que funcione con software heredado/antiguo". Si lo desea, puede sustituir esa lista con el contenido del siguiente bloque de código de ejemplo.

    La elección de qué configuración usar dependerá en gran medida de lo que necesite admitir. Ambos proporcionarán una gran seguridad.

Para sus propósitos, copie la configuración proporcionada en su totalidad, pero primero deberá realizar algunas modificaciones pequeñas.

Primero, agregue su resolución de DNS preferida para las solicitudes ascendentes. Usaremos (``8.8.8.8`` y ``8.8.4.4``) de Google para esta guía.

En segundo lugar, comente la línea que establece el encabezado de seguridad de transporte estricta. Antes de quitar el comentario de esta línea, debe tomarse un momento para leer sobre la `[seguridad de transporte estricta de HTTP, o HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security) , y específicamente sobre la f[uncionalidad de "precarga"](https://hstspreload.appspot.com/). La precarga de HSTS proporciona una mayor seguridad, pero también puede tener consecuencias negativas de gran alcance si se habilita accidentalmente o de forma incorrecta.

Agregue lo siguiente en su archivo ``ssl-params.conf`` de fragmento:

``` nginx title="/etc/nginx/snippets/ssl-params.conf"
ssl_protocols TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/nginx/dhparam.pem; 
ssl_ciphers EECDH+AESGCM:EDH+AESGCM;
ssl_ecdh_curve secp384r1;
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable strict transport security for now. You can uncomment the following
# line if you understand the implications.
#add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

Debido a que está utilizando un certificado autofirmado, no se utilizará el engrapado SSL. Nginx generará una advertencia y deshabilitará el engrapado de nuestro certificado autofirmado, pero luego seguirá funcionando correctamente.

Guarde y cierre el archivo cuando haya terminado.

#### Ajuste de la configuración de Nginx para usar SSL

Ahora que tiene sus fragmentos, puede ajustar la configuración de Nginx para habilitar SSL.

Supondremos en esta guía que está utilizando un archivo de configuración de bloque de servidor personalizado en el directorio ``/etc/nginx/sites-available``. Esta guía también sigue las convenciones del tutorial de Nginx de requisitos previos y se usa para este ejemplo. Sustituya su nombre de archivo de configuración según sea necesario ``./etc/nginx/sites-available/weppor.desarrollo``.

Antes de continuar, haga una copia de seguridad de su archivo de configuración actual:

``` shell
sudo cp /etc/nginx/sites-available/weppor.desarrollo /etc/nginx/sites-available/weppor.desarrollo.bak
```

Ahora, abra el archivo de configuración para hacer ajustes:
``` shell
sudo nano /etc/nginx/sites-available/weppor.desarrollo
```

En el interior, el bloque de su servidor probablemente comience de manera similar a lo siguiente:

``` nginx title="/etc/nginx/sitios-disponibles/weppor.desarrollo"
server {
        listen 80;
        listen [::]:80;

        root /var/www/weppor.desarrollo/html;
        index index.html index.htm index.nginx-debian.html;

        server_name weppor.desarrollo www.weppor.desarrollo;

        location / {
                try_files $uri $uri/ =404;
        }
}
```

Su archivo puede estar en un orden diferente y, en lugar de las directivas ``root`` e ``index``, puede tener algunas declaraciones de configuración ``location proxy_pass`` personalizadas, u otras. Esto está bien ya que solo necesita actualizar las directivas ``listen`` e incluir los fragmentos de SSL. Luego, modifique este bloque de servidor existente para atender el tráfico SSL en el puerto ``443`` y cree un nuevo bloque de servidor para responder en el puerto ``80`` y redirigir automáticamente el tráfico al puerto ``443``.

!!! note "Nota"
     Utilice una redirección 302 hasta que haya verificado que todo funciona correctamente. Después, cambiará esto a una redirección permanente 301.

En su archivo de configuración existente, actualice las dos declaraciones ``listen`` para usar port ``443`` y ``ssl``, luego incluya los dos archivos de fragmentos que creó en los pasos anteriores:

``` nginx title="/etc/nginx/sitios-disponibles/weppor.desarrollo"
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
```

A continuación, agregue un segundo bloque de servidor al archivo de configuración después del corchete de cierre (``}``) del primer bloque:

``` nginx title="/etc/nginx/sitios-disponibles/weppor.desarrollo"

server {
    listen 80;
    listen [::]:80;

    server_name weppor.desarrollo www.weppor.desarrollo;

    return 302 https://$server_name$request_uri;
}
```

Esta es una configuración básica que escucha en el puerto ``80`` y realiza la redirección a HTTPS. Guarde y cierre el archivo cuando haya terminado de editarlo.

### Paso 3: Ajustar el cortafuegos
Si tiene ``ufw`` habilitado el firewall, como se recomienda en la guía de requisitos previos, deberá ajustar la configuración para permitir el tráfico SSL. Afortunadamente, Nginx registra algunos perfiles en ``ufw`` el momento de la instalación.

!!! warning "Aviso"
    En la guía previa ya se permitió el acceso a Nging Full, con lo que puede saltar este paso.

Puede revisar los perfiles disponibles escribiendo:
``` shell
sudo ufw app list
Aparecerá una lista como la siguiente:
```

``` Output
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

Puede comprobar la configuración actual escribiendo ``sudo ufw status``:

``` shell
sudo ufw status
```

Probablemente generará la siguiente respuesta, lo que significa que solo se permite el tráfico HTTP al servidor web:

``` Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

Para permitir el tráfico HTTPS, puede actualizar los permisos para el perfil ``"Nginx Full"`` y luego eliminar la asignación redundante del perfil ``"Nginx HTTP"``:
``` shell
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```

Después de ejecutar ``sudo ufw status``, debería recibir el siguiente resultado:

``` shell
sudo ufw status
```

``` Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
```

Este resultado confirma que los ajustes a su firewall fueron exitosos y que está listo para habilitar los cambios en Nginx.

### Paso 4: habilitar los cambios en Nginx
Con los cambios y ajustes en su firewall completos, puede reiniciar Nginx para implementar los nuevos cambios.

Primero, verifique que no haya errores de sintaxis en los archivos. Puede hacer esto escribiendo ``sudo nginx -t``:
``` shell
sudo nginx -t
```

Si todo es exitoso, obtendrá un resultado que dice lo siguiente:

``` Output
nginx: [warn] "ssl_stapling" ignored, issuer certificate not found for certificate "/etc/ssl/certs/nginx-selfsigned.crt"
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

Fíjate en la advertencia al principio. Como se señaló anteriormente, esta configuración en particular genera una advertencia ya que su certificado autofirmado no puede usar el engrapado SSL. Esto es de esperar y su servidor aún puede cifrar las conexiones correctamente.

Si su salida coincide con nuestro ejemplo, su archivo de configuración no tiene errores de sintaxis. Si este es el caso, puede reiniciar Nginx de manera segura para implementar los cambios:

``` shell
sudo systemctl restart nginx
```

Ahora que el sistema se ha reiniciado con los nuevos cambios, puede continuar con la prueba.

### Paso 5: Probar el cifrado
Ahora, está listo para probar su servidor SSL.

Abra su navegador web y escriba ``https://`` seguido del nombre de dominio o IP de su servidor en la barra de direcciones:

[https://weppor.desarrollo](https://weppor.desarrollo)

Dependiendo de su navegador, es probable que reciba una advertencia ya que el certificado que creó no está firmado por una de las autoridades de certificación de confianza de su navegador:

Esta advertencia es esperada y normal. Solo nos interesa el aspecto de cifrado de nuestro certificado, no la validación de la autenticidad de nuestro host por parte de terceros. Haga clic en **"AVANZADO"** y luego en el enlace provisto para proceder a su host:

En este punto, debe ser llevado a su sitio. En nuestro ejemplo, la barra de direcciones del navegador muestra un candado con una "x" encima, lo que significa que el certificado no se puede validar. Todavía está cifrando su conexión. Tenga en cuenta que este icono puede diferir, dependiendo de su navegador.

Si configuró Nginx con dos bloques de servidor, redirigiendo automáticamente el contenido HTTP a HTTPS, también puede verificar si la redirección funciona correctamente:

[http://weppor.desarrollo](http://weppor.desarrollo)

Si esto da como resultado el mismo ícono, significa que su redirección funcionó correctamente.

### Paso 6: cambiar a una redirección permanente
Si su redirección funcionó correctamente y está seguro de que desea permitir solo el tráfico cifrado, debe modificar la configuración de Nginx para que la redirección sea permanente.

Abra de nuevo el archivo de configuración del bloque del servidor:
``` shell
sudo nano /etc/nginx/sites-available/weppor.desarrollo
```

Encuentra el ``return 302`` y cámbialo a ``return 301``:

``` nginx title="/etc/nginx/sitios-disponibles/weppor.desarrollo"
	return 301 https://$server_name$request_uri;
```

Guarde y cierre el archivo.

Verifique su configuración en busca de errores de sintaxis:

``` shell
sudo nginx -t
```

Cuando esté listo, reinicie Nginx para que la redirección sea permanente:

``` shell
sudo systemctl restart nginx
```

Después del reinicio, se implementarán los cambios y su redirección ahora es permanente.

### Conclusión
Configuró su servidor Nginx para usar un cifrado fuerte para las conexiones de los clientes. Esto le permitirá atender las solicitudes de forma segura y evitará que terceros externos lean su tráfico. Como alternativa, puede optar por utilizar un certificado SSL autofirmado que se puede obtener de Let's Encrypt, una autoridad de certificación que instala certificados TLS/SSL gratuitos y habilita HTTPS cifrado en servidores web. Obtenga más información de nuestro tutorial sobre [Cómo proteger Nginx con Let's Encrypt en Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04).