# plone/plone-frontend

Este capítulo cubre la imagen de Docker de interfaz predeterminada de Plone 6 usando Node. El frontend está escrito usando React y requiere un backend de Plone para ejecutarse y ser accesible.

!!! info "Nota"
    Esta imagen no es una imagen base para ser extendida en sus proyectos, sino un ejemplo de la experiencia de usuario de Plone lista para usar.

## Variables de configuración

### Variables principales

| Variable de entorno      | Descripción                                                                  | Ejemplo                 |
|--------------------------|------------------------------------------------------------------------------|-------------------------|
| RAZZLE_API_PATH          | Se utiliza para generar llamadas de frontend al backend. Debe ser una URL pública a la que pueda acceder el navegador del cliente. | ``http://api.site.org/++api++/`` |
| RAZZLE_INTERNAL_API_PATH | Utilizado por el middleware para construir solicitudes al backend. Puede ser una dirección no pública.           | ``http://backend:8080/Plone``    |
| VOLTO_ROBOTSTXT          | Anule el archivo robots.txt.                                                                          | ``"User-agent: *\nDisallow: "``  |

!!! note "Nota"

    Para obtener una lista completa de las variables de entorno utilizadas por la interfaz, visite [Variables de entorno](https://6.docs.plone.org/volto/configuration/environmentvariables.html).

## Uso como ejemplo para su proyecto Volto

Para usar esta imagen como ejemplo de una imagen Docker para su propio proyecto Volto, deberá editar el archivo ``Dockerfile`` en su proyecto. ``Dockerfile`` se extrae de la raíz del repositorio [plone/plone-frontend](https://github.com/plone/plone-frontend/)

!!! note "Nota"

    Los ejemplos de ``Dockerfile`` en esta documentación usan Volto 15.x. Es posible que deba adaptar los ejemplos para versiones más recientes.


En Dockerfile, reemplace el comando ``yo @plone/volto`` con el comando ``COPY /build/plone-frontend``.

``` diff title="Dockerfile"

   # Generar un nueva app volto
-  RUN yo @plone/volto \
+  RUN COPY . /build/plone-frontend \
+     plone-frontend \

```

### Crear un punto de entrada personalizado

La imagen de Docker ``plone-frontend`` no tiene un archivo de punto de entrada personalizado. Para cualquier comando que necesite ejecutar al iniciar su contenedor Docker, deberá crearlo.

Después de crear el archivo ``entrypoint.sh``, asegúrese de que tenga el permiso de ejecución:

``` shell

chmod 755 entrypoint.sh

```

!!! note "Importante"
    No olvide agregar el comando exec "$@" al final del archivo ``entrypoint.sh`` para ejecutar el comando predeterminado ``yarn start``

En ``Dockerfile``, deberá agregar dos comandos para que el contenedor Docker ejecute ``entrypoint.sh`` al inicio:

``` diff title="Dockerfile"

  --no-interactive
+
+ COPY entrypoint.sh /

  RUN cd plone-frontend \

```

``` diff title="Dockerfile"

  CMD ["yarn", "start:prod"]
+ ENTRYPOINT ["/entrypoint.sh"]

```

### Construir
Construya la nueva imagen

``` shell
docker build . -t myfrontend:latest -f Dockerfile
```


### Arrancarlo
Puede usarlo en el siguiente archivo ```docker-compose.yml```

``` yaml title="docker-compose.yml"

version: "3"
services:

  backend:
    image: plone/plone-backend:6.0
    # Plone 5.2 series can be used too
    # image: plone/plone-backend:5.2.7
    ports:
      - '8080:8080'
    environment:
      - SITE=Plone
      - 'ADDONS=plone.restapi==8.21.0 plone.volto==4.0.0a3 plone.rest==2.0.0a2 plone.app.iterate==4.0.2 plone.app.vocabularies==4.3.0'
      - 'PROFILES=plone.volto:default-homepage'

  frontend:
    image: 'myfrontend:latest'
    ports:
      - '3000:3000'
    restart: always
    environment:
      # These are needed in a Docker environment since the
      # routing needs to be amended. You can point to the
      # internal network alias.
      RAZZLE_INTERNAL_API_PATH: http://backend:8080/Plone
      RAZZLE_DEV_PROXY_API_PATH: http://backend:8080/Plone
    depends_on:
      - backend

```

Para arrancar, ejecute el siguiente comando:

``` shell
docker compose up -d
```

## Versiones


Para obtener una lista completa de etiquetas y versiones, visite la página [plone/plone-frontend en Docker Hub](https://hub.docker.com/r/plone/plone-frontend).