---
myst:
  html_meta:
    "description": "Very simple Plone 6 setup with only one backend and data being persisted in a Docker volume."
    "property=og:description": "Very simple Plone 6 setup with only one backend and data being persisted in a Docker volume."
    "property=og:title": "nginx, Frontend, Backend container example"
    "keywords": "Plone 6, Container, Docker, nginx, Frontend, Backend"
---

# Contenedor ejemplo con nginx, Frontend, Backend y Volto

Este ejemplo es una configuración muy simple con un backend y persistiendo los datos en un volumen Docker.

`nginx`, en este ejemplo, se usa como [proxy inverso](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).

## Configuración

Crear un directorio de proyecto vacío con el nombre `nginx-volto-plone`

```shell
mkdir nginx-volto-plone
```

Cambiar al directorio de proyecto

```shell
cd nginx-volto-plone
```


### Configuración nginx

Añadir un archivo `default.conf` que será usado por la imagen `nginx`


```nginx
upstream backend {
  server backend:8080;
}
upstream frontend {
  server frontend:3000;
}

server {
  listen 80  default_server;
  server_name  plone.localhost;

  location ~ /\+\+api\+\+($|/.*) {
      rewrite ^/(\+\+api\+\+\/?)+($|/.*) /VirtualHostBase/http/$server_name/Plone/++api++/VirtualHostRoot/$2 break;
      proxy_pass http://backend;
  }

  location ~ / {
      location ~* \.(js|jsx|css|less|swf|eot|ttf|otf|woff|woff2)$ {
          add_header Cache-Control "public";
          expires +1y;
          proxy_pass http://frontend;
      }
      location ~* static.*\.(ico|jpg|jpeg|png|gif|svg)$ {
          add_header Cache-Control "public";
          expires +1y;
          proxy_pass http://frontend;
      }

      proxy_set_header        Host $host;
      proxy_set_header        X-Real-IP $remote_addr;
      proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header        X-Forwarded-Proto $scheme;
      proxy_redirect http:// https://;
      proxy_pass http://frontend;
  }
}
```


!!! note "Nota"

    `http://plone.localhost/` es la URL que usarás para acceder al sitio web.
    Puedes usar `localhost` o, bien añadirlo al archivo  `/etc/hosts` o bien a un DNS para apuntar a la IP del Host Docker.


### Configuración del Servicio con Docker Compose

Crearemos ahora un archivo `docker-compose.yml`:

```yaml
version: "3"
services:

  webserver:
    image: nginx
    volumes:
      - ./default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - backend
      - frontend
    ports:
    - "80:80"

  frontend:
    image: plone/plone-frontend:latest
    environment:
      RAZZLE_INTERNAL_API_PATH: http://backend:8080/Plone
    ports:
    - "3000:3000"
    depends_on:
      - backend

  backend:
    image: plone/plone-backend:{PLONE_BACKEND_MINOR_VERSION}
    environment:
      SITE: Plone
    volumes:
      - data:/data
    ports:
    - "8080:8080"

volumes:
  data: {}
```


## Generar el proyecto

Construir el stack con el comando `docker compose`.

```shell
docker compose up -d
```

Esto obtiene las imágenes necesarias y arranca Plone.


## Acceder a Plone con el navegador

Después del arranque, vaya a `http://plone.localhost/` y debería ver el sitio.


## Apagado y limpieza

El comando `docker compose down` elimina los contenedores y la red por defecto, pero mantiene la base de datos plone.

El comando `docker compose down --volumes` elimina los contenedores, la red por defecto y la base de datos plone, eliminado el volumen.