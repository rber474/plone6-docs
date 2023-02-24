---
myst:
  html_meta:
    "description": "Using plone/plone-zeo image"
    "property=og:description": "Using plone/plone-zeo image"
    "property=og:title": "Plone ZEO image"
    "keywords": "Plone 6, install, installation, docker, containers, plone/plone-zeo"
---

# plone/plone-zeo

Una imagen de ZEO Server [Docker](https://www.docker.com/) usando Python 3 y  [pip](https://pip.pypa.io/en/stable/).


## Usando esta imagen


### Uso sencillo

Ejecute un contenedor que exponga el puerto 8100:

```shell
docker run -p 8100:8100 plone/plone-zeo:latest
```


### Configuración del servicio con Docker Compose

Cree un directorio para su proyecto y dentro de él cree un archivo `docker-compose.yml` que inicie su instancia de Plone y la instancia de ZEO con montajes de volumen para la persistencia de datos:

```yaml title="docker-compose.yml"

version: "3"
services:

  backend:
    image: plone/plone-backend:latest
    environment:
      ZEO_ADDRESS: zeo:8100
    ports:
    - "8080:8080"
    depends_on:
      - zeo

  zeo:
    image: plone/plone-zeo:latest
    volumes:
      - data:/data
    ports:
    - "8100:8100"

volumes:
  data: {}
```

Ahora, ejecute `docker compose up -d` desde el directorio de su proyecto.

Dirija su navegador a http://localhost:8080 y debería ver la página predeterminada de creación de sitios de Plone.

### Datos persistentes

Hay varias formas de almacenar datos utilizados por aplicaciones que se ejecutan en contenedores Docker.

nimamos a los usuarios de las imágenes `Plone` ia familiarizarse con las opciones disponibles.

[La documentación de Docker](https://docs.docker.com/) es un buen punto de partida para comprender las diferentes opciones y variaciones de almacenamiento.


## Configuración


### Variables principales

| Variable de entorno | Opción ZEO | Valor por defecto |
| --- | --- | --- |
| `ZEO_PORT` | `address` | `8100` |
| `ZEO_READ_ONLY` | `read-only` | `false` |
| `ZEO_INVALIDATION_QUEUE_SIZE` | `invalidation-queue-size` | `100` |
| `ZEO_PACK_KEEP_OLD` | `pack-keep-old` | `true` |

En caso de que necesite configurar una opción no presente en las variables de entorno, le sugerimos que cree una nueva imagen basada en la predeterminada:

```Dockerfile
FROM plone/plone-zeo:latest

COPY mylocalconfig /app/etc/zeo.conf
```
Y luego construya la nueva imagen e inicie el contenedor.


## Versions

Para obtener una lista completa de etiquetas y versiones, visite la [pa´gina `plone/plone-zeo` en Docker Hub](https://hub.docker.com/r/plone/plone-zeo).
