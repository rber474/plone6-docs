# plone/plone-backend

Este capítulo cubre las imágenes de [Docker](https://www.docker.com/) backend de Plone usando Python 3 y [pip](https://pip.pypa.io/en/stable/).

## Usando esta imagen

### Uso simple

```
docker run -p 8080:8080 plone/plone-backend:6.0 start
```

Luego apunte su navegador a [http://localhost:8080](http://localhost:8080). Debería ver la página predeterminada de creación de sitios de Plone.

### Datos persistentes

Hay varias formas de almacenar datos utilizados por aplicaciones que se ejecutan en contenedores [Docker](https://www.docker.com/).

Alentamos a los usuarios de las ``imágenes de Plone`` a familiarizarse con las opciones disponibles.

La documentación de Docker es un buen punto de partida para comprender las diferentes opciones y variaciones de almacenamiento.

## Variables de configuración

### Variables principales

| Variable de entorno            | Opción en Zope                 | Valor por defecto     |
|--------------------------------|--------------------------------|-----------------------|
| DEBUG_MODE                     | debug-mode                     | ``off``               |
| SECURITY_POLICY_IMPLEMENTATION | security-policy-implementation | ``C``                 |
| VERBOSE_SECURITY               | verbose-security               | ``false``             |
| DEFAULT_ZPUBLISHER_ENCODING    | default-zpublisher-encoding    | ``utf-8``             |
| LISTEN_PORT                    | (sin equivalencia)             | ``8080``              |


#### Listen Port (Puerto de servicio)

De manera predeterminada, el proceso de Zope dentro del contenedor escuchará en el puerto TCP 8080. En ciertas circunstancias (Kubernetes o pods de Podman), puede ser necesario ejecutar más de un proceso de Zope dentro del espacio de nombres de la red, lo que daría lugar a conflictos de puerto de escucha como dos procesos diferentes dentro del mismo espacio de nombres intentan escuchar el mismo puerto TCP.

En estos casos, la variable ```LISTEN_PORT``` se puede establecer en cualquier puerto en particular por encima de 1024 para garantizar que el contenedor escuche en el puerto deseado.

Ejemplo para Plone 6:

``` bash

# Hace que Zope escuche por el puerto 8081 en vez del 8080.
docker run -p 8081:8081 -e LISTEN_PORT=8081 plone/plone-backend:6.0

```

### Variables para la creación de un sitio

| Variable de entorno  | Descripción                                                                           |
|----------------------|---------------------------------------------------------------------------------------|
| SITE                 | Id del sitio que será creado, por ejemplo, ``Plone``                                  |
| TYPE                 | Tipo de sitio, bien ``classic`` o ```volto```. Por defecto: ``volto``                 |
| PROFILES             | Inicializa el sitio con perfiles adicionales, por ejemplo, ``eea.api.layout:default`` |
| DELETE_EXISTING      | Fuerza a recrear el sitio si este ya existe, por ejemplo, ``true``                    |


Es posible inicializar su base de datos con una instancia de ``Plone Site`` en su primera ejecución. Para hacerlo, pase la variable de entorno ``SITE`` con el nombre de la instancia del sitio Plone, por ejemplo, ``SITE=Plone``. Esto agregará un sitio Plone listo para Volto. Si desea una instancia de IU de Plone Classic, pase la variable de entorno y el valor ``TYPE=classic``. Para inicializarlo con perfiles adicionales, páselos como valores separados por espacios a través de la variable de entorno ``PROFILES``, por ejemplo, ``PROFILES=eea.api.layout:default``. Para recrear el sitio de Plone al reiniciar el contenedor, puede pasar la variable de entorno ``DELETE_EXISTING``.

Ejemplo Plone 6:
``` shell

docker run -p 8080:8080 -e ADDONS="eea.api.layout" -e SITE="Plone" -e PROFILES="eea.api.layout:default" plone/plone-backend:6.0

```

Ejemplo para Plone 6 Classic UI:
``` shell

docker run -p 8080:8080 -e ADDONS="eea.facetednavigation" -e SITE="Plone" -e TYPE="classic" -e PROFILES="eea.facetednavigation:default" plone/plone-backend:6.0

```

!!! warning "Recomendación"
    Recomendamos no utilizar esta función en entornos de producción.

### Variables ZOPE

| Variable de entorno  | Descripción                | Valor por defecto     |
|----------------------|----------------------------|-----------------------|
| ZODB_CACHE_SIZE      | Tamaño caché base de datos | 50000                 |


### Variables ZEO

| Variable de entorno           | Descripción                         | Opción ZEO               | Valor por defecto     |
|-------------------------------|-------------------------------------|--------------------------|-----------------------|
| ZEO_ADDRESS                   | URL del interfaz ZEO, host:port     |                          |                       |
| ZEO_SHARED_BLOB_DIR           | Opción ZEO                          | name                     | ``off``               |
| ZEO_READ_ONLY                 | Opción ZEO                          | read-only                | ``false``             |
| ZEO_CLIENT_READ_ONLY_FALLBACK | Opción ZEO                          | read-only-fallback       | ``false``             |
| ZEO_STORAGE                   | Opción ZEO                          | storage                  | ``1``                 |
| ZEO_CLIENT_CACHE_SIZE         | Opción ZEO                          | cache-size               | ``128MB``             |
| ZEO_DROP_CACHE_RATHER_VERIFY  | Opción ZEO                          | drop-cache-rather-verify | ``false``             |

#### Ejemplo

``` yaml title="docker-compose.yaml"

version: "3"
services:

  backend:
    image: plone/plone-backend:6.0
    restart: always
    environment:
      ZEO_ADDRESS: zeo:8100
    ports:
    - "8080:8080"
    depends_on:
      - zeo

  zeo:
    image: plone/plone-zeo:latest
    restart: always
    volumes:
      - data:/data
    ports:
    - "8100:8100"

volumes:
  data: {}

```

### Variables para BBDD Relacionales

| Variables de entorno                      | Descripción                               | opción RelStorage              | Valor por defecto |
|-------------------------------------------|-------------------------------------------|--------------------------------|---------------|
| RELSTORAGE_DSN                            | [PostgreSQL DSN](postgresql-dsn) para la conexión |                                |               |
| RELSTORAGE_NAME                           | opción RelStorage                         | name                           | ``storage``       |
| RELSTORAGE_READ_ONLY                      | opción RelStorage                         | read-only                      | ``off``           |
| RELSTORAGE_KEEP_HISTORY                   | opción RelStorage                         | keep-history                   | ``true``          |
| RELSTORAGE_COMMIT_LOCK_TIMEOUT            | opción RelStorage                         | commit-lock-timeout            | ``30``            |
| RELSTORAGE_CREATE_SCHEMA                  | opción RelStorage                         | create-schema                  | ``true``          |
| RELSTORAGE_SHARED_BLOB_DIR                | opción RelStorage                         | shared-blob-dir                | ``false``         |
| RELSTORAGE_BLOB_CACHE_SIZE                | opción RelStorage                         | blob-cache-size                | ``100mb``         |
| RELSTORAGE_BLOB_CACHE_SIZE_CHECK          | opción RelStorage                         | blob-cache-size-check          | ``10``            |
| RELSTORAGE_BLOB_CACHE_SIZE_CHECK_EXTERNAL | opción RelStorage                         | blob-cache-size-check-external | ``false``         |
| RELSTORAGE_BLOB_CHUNK_SIZE                | opción RelStorage                         | blob-chunk-size                | ``1048576``       |
| RELSTORAGE_CACHE_LOCAL_MB                 | opción RelStorage                         | cache-local-mb                 | ``10``            |
| RELSTORAGE_CACHE_LOCAL_OBJECT_MAX         | opción RelStorage                         | cache-local-object-max         | ``16384``         |
| RELSTORAGE_CACHE_LOCAL_COMPRESSION        | opción RelStorage                         | cache-local-compression        | ``none``          |
| RELSTORAGE_CACHE_DELTA_SIZE_LIMIT         | opción RelStorage                         | cache-delta-size-limit         | ``100000``        |

!!! info "Nota"

    Actualmente, esta imagen solo admite la configuración de un backend de PostgreSQL a través de variables de configuración. Si necesita usar MySQL u Oracle, le recomendamos que amplíe esta imagen y sobrescriba el archivo ``/app/etc/relstorage.conf``.

#### PostgreSQL DSN

Un DSN de PostgreSQL válido es una lista de parámetros separados por espacios en blanco. Un DSN típico tiene el siguiente aspecto:

```
dbname='zodb' user='username' host='localhost' password='pass'
```

#### Ejemplo
``` yaml title="docker-compose.yaml"

version: "3"
services:

  backend:
    image: plone/plone-backend:6.0
    environment:
      RELSTORAGE_DSN: "dbname='plone' user='plone' host='db' password='plone'"
    ports:
    - "8080:8080"
    depends_on:
      - db

  db:
    image: postgres
    environment:
      POSTGRES_USER: plone
      POSTGRES_PASSWORD: plone
      POSTGRES_DB: plone
    ports:
    - "5432:5432"

```
### Variables CORS
| Variables de entorno   | Descripción                                                                                                                                        | Valor por defecto                                     |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
| CORS_ALLOW_ORIGIN      | Origenes con acceso permitido al recurso. Puede ser una lista separada por comas o *. Por ejemplo: ``http://example.net,http://mydomain.com`` or ``*`` | ``http://localhost:3000,http://127.0.0.1:3000``       |
| CORS_ALLOW_METHODS     | Una lista separadas por comas de métodos HTTP que se permiten por esta política CORS, por ejemplo DELETE,GET,OPTIONS,PATCH,POST,PUT                    | ``DELETE,GET,OPTIONS,PATCH,POST,PUT``                 |
| CORS_ALLOW_CREDENTIALS | Indica si el recurso soporta credenciales de usuario en la REQUEST                                                                            | ``true``                                              |
| CORS_EXPOSE_HEADERS    | Una lista separadas por comas de encabezados de la RESPONSE que los clientes pueden acceder, por ejemplo Content-Length,X-My-Header                                              | ``Content-Length,X-My-Header``                        |
| CORS_ALLOW_HEADERS     | Una lista separadas por comas de encabezados de la REQUEST permitidos para que puedan ser enviados por el cliente, por ejemplo X-My-Header                                                | ``Accept,Authorization,Content-Type,X-Custom-Header`` |
| CORS_MAX_AGE           | Indica cuánto tiempo se pueden almacenar en caché los resultados de una solicitud de verificación previa                                                                                | ``3600``                                              |

These variables are used to configure [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

### Complementos (Add-ons)

Es posible incluir complementos durante el tiempo de inicio en un contenedor creado con esta imagen.

!!! warning "Aviso"

    Recomendamos no utilizar esta función en entornos de producción: el método recomendado es ampliar las imágenes oficiales del contenedor para incluir los complementos que desee en su propio contenedor. Esto tiene varias ventajas, entre las que se encuentra la certeza de que su contenedor siempre ejecutará el código adicional exacto que incorporó.

Para hacerlo, pase la variable de entorno ``ADDONS`` con una lista de requisitos separados por espacios para agregar a la imagen (consulte a continuación la documentación de las variables admitidas):

``` shell
docker run -p 8080:8080 -e ADDONS="pas.plugins.authomatic" plone/plone-backend:6.0 start
```


Una vez que se haya iniciado Plone, puede agregar su sitio Plone (si aún no existe) e instalar los complementos agregados a su sitio.

Este enfoque también le permite probar Plone con una versión específica de uno de sus componentes principales.

``` shell
docker run -p 8080:8080 -e ADDONS="plone.volto==3.1.0a3" plone/plone-backend:6.0 start
```


#### Variables para los complementos

| Variables de entorno | Descripción                                                                             | Detalles                     |
|----------------------|-----------------------------------------------------------------------------------------|------------------------------|
| ADDONS               | Una lista separada por espacios de bibliotecas de python para instalar                  | [Complementos](#complementos-add-ons)                      |
| DEVELOP              | Una lista separada por espacios de bibliotecas de python para instalar en modo editable | [Variable de paquetes de desarrollo](#variable-de-paquetes-de-desarrollo) |
| PIP_PARAMS           | Parámetros utilizados en los comandos de instalación de pip                             | [pip install](https://pip.pypa.io/en/stable/cli/pip_install/)                  |


#### Añadir configuración a ``zope.conf`` o ``zcml-additional``

Algunos complementos de Plone requieren cambios en ``zope.conf`` o ZCML adicional (``zcml-additional``).

Con el contenedor estándar, no es posible agregar fragmentos de configuración a ``zope.conf`` directamente o agregar ZCML adicional (``zcml-additional``), como ocurre con el método de implementación **``buildout``**.

Sin embargo, puede derivar su propia imagen de contenedor y colocar fragmentos de configuración. Consulte [Ampliación de esta imagen](ampliacion-de-esta-imagen) a continuación para obtener instrucciones.

### Variable de paquetes de desarrollo

Es posible instalar paquetes locales en lugar de paquetes de ``pip``. Para hacerlo, pase la variable de entorno ``DEVELOP`` con una lista de rutas separadas por espacios a los paquetes de Python que se instalarán. Estos paquetes se instalarán con ``pip install --editable``.

!!! warning "Aviso"

    Recomendamos no utilizar esta función en entornos de producción: el método recomendado es ampliar las imágenes oficiales del contenedor para incluir los complementos que desee en su propio contenedor.

``` shell
docker run -p 8080:8080 -e DEVELOP="/app/src/mysite.policy" plone/plone-backend:6.0 start
```

Este enfoque también le permite desarrollar paquetes locales mediante el uso de un volumen.
``` shell
docker run -p 8080:8080 -e DEVELOP="/app/src/mysite.policy" -v /path/to/mysite.policy:/app/src/mysite.policy plone/plone-backend:6.0 start
```

## Ampliación de esta imagen

En un directorio, cree un archivo ``Dockerfile``:

``` Dockerfile title="Dockerfile"

FROM plone/plone-backend:6.0

RUN apt-get update \
    && apt-get install -y --no-install-recommends gcc \
    && rm -rf /var/lib/apt/lists/*

```


Construya la nueva imagen:

``` shell
docker build . -t myproject:latest -f Dockerfile
```

Y arranque un contenedor:
``` shell
docker run -p 8080:8080 myproject:latest start
```

#### Cambiar los valores predeterminados de las variables de entorno

Todas las variables de entorno documentadas anteriormente son compatibles con el Dockerfile de su contenedor derivado. Puede anular los valores predeterminados de las variables de la siguiente manera:

``` Dockerfile title="Dockerfile"
# Agregue variables de entorno antes de cualquier comando CMD o ENTRYPOINT,
# y después de los comandos FROM, RUN y COPY.
ENV ZODB_CACHE_SIZE="120000"
```

Por supuesto, siempre puede anular estas variables al iniciar el contenedor utilizando el argumento de ejecución de docker de Docker ```-e VAR=value```.

!!! info "Nota"
    Tenga en cuenta que algunas variables no están destinadas a ser utilizadas en producción. Consulte la documentación de la variable respectiva anterior para determinar si debe usarla o usar un método diferente para obtener el resultado deseado en la producción.

#### Añadir de fragmentos de configuración de ``zope.conf``

En el directorio que contiene su ``Dockerfile``, cree una carpeta ``etc/zope.conf.d``. Agregue sus fragmentos de configuración de ``zope.conf`` allí.

Ahora agregue lo siguiente a su ``Dockerfile``, antes de cualquier comando ``CMD`` o ``ENTRYPOINT`` que pueda tener, y después de los comandos ``FROM`` y ``RUN``:

``` Dockerfile title="Dockerfile"

COPY /etc/zope.conf.d/*.conf /app/etc/zope.conf.d/

```
Esto asegura que sus fragmentos se implementen en la carpeta ``zope.conf.d``, que luego se usará para modificar el archivo ``zope.conf`` antes de iniciar Plone.


#### Añadir de fragmentos ZCML

En el directorio que contiene su ``Dockerfile``, cree una carpeta ``etc/package-includes``. Agregue sus fragmentos de configuración ZCML (llamados ``*-meta.zcml``, ``*-configure.zcml``, ``*-overrides.zcml``) como archivos en esa carpeta.

Ahora agregue lo siguiente a su ``Dockerfile``, antes de cualquier comando ``CMD`` o ``ENTRYPOINT`` que pueda tener, y después de los comandos ``FROM`` y ``RUN``:

``` Dockerfile title="Dockerfile"

COPY /etc/package-includes/*.zcml /app/etc/package-includes/

```
Sus fragmentos ZCML se copiarán en el contenedor y se incluirán automáticamente cuando se inicie Plone.

## Uso avanzado

### ``--user`` arbitrario

Esta imagen admite la ejecución como un usuario (en su mayoría) arbitrario a través de ``--user`` on docker run, siempre que el propietario de ``/data`` coincida:

``` shell

docker run --user="$(id -u)" -v $(pwd)/data:/data plone/plone-backend
```

La advertencia principal a tener en cuenta es que algunas variables de entorno, como ``ADDONS`` y ``DEVELOP``, no funcionarán:

``` shell

docker run --user="$(id -u)" -v $(pwd)/data:/data -e ADDONS="eea.facetednavigation" plone/plone-backend
...
error: [Errno 13] Permission denied: '/app/lib/python3.9/site-packages/eea'
```

### Múltiples contenedores con ZEO
Esta imagen admite clústeres ZEO como una forma sencilla de permitir el escalado horizontal del backend. Consulte la página de ejemplo: [Variables de entorno](#variables-principales).

## Versiones
Para obtener una lista completa de etiquetas y versiones, visite la página [plone/plone-backend en Docker Hub](https://hub.docker.com/r/plone/plone-backend).

