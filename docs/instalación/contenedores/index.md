# Contenedores

Usar contenedores es la manera más sencilla de probar y desplegar Plone 6.

## Empezando

!!! info "Nota"

    Aunque existen muchas herramientas de motor de contenedores para desarrollar, administrar y ejecutar contenedores, usaremos Docker en esta documentación.

## Requisitos de sistema

!!! todo "Por hacer..."

    Añadir los requisitos del sistema

## Instalar Docker

Instale [Docker](https://docs.docker.com/get-docker/) para su sistema operativo

Docker Desktop incluye todas las herramientas de Docker. Tanto macOS como Windows usan Docker Desktop. En algunas distribuciones de Linux está disponible una versión beta de Docker Desktop. Si Docker Desktop no está disponible para su distribución de Linux, aún puede instalar Docker Engine y todas sus herramientas. Consulte [Obtener Docker](https://docs.docker.com/get-docker/) para obtener más detalles.

[Docker Compose](https://6.docs.plone.org/glossary.html#term-Docker-Compose) es una de las herramientas de Docker que se utilizará a lo largo de esta documentación.

## Arrancar Plone

Primero inicie Plone Backend, llámelo ``plone6-backend`` y cree un sitio con su configuración predeterminada, usando el siguiente comando:
```
docker run --name plone6-backend -e SITE=Plone -e CORS_ALLOW_ORIGIN='*' -d -p 8080:8080 plone/plone-backend:6.0
```

Ahora inicie el Plone Frontend, enlazandolo con el ``plone6-backend``:
```
docker run --name plone6-frontend --link plone6-backend:backend -e RAZZLE_API_PATH=http://localhost:8080/Plone -e RAZZLE_INTERNAL_API_PATH=http://backend:8080/Plone -d -p 3000:3000 plone/plone-frontend:latest
```

## Acceder a su sitio Plone

Acceda con su navegador a la URL ``http://localhost:3000`` and debería ver el nuevo sitio plone.

!!! info "Nota"

    El usuario por defecto es **``admin``**  y la clave es **``admin``**

## Parada y limpieza

Para detener y eliminar los contenedores, use los siguientes comandos:
```
docker stop plone6-frontend && docker rm plone6-frontend
docker stop plone6-backend && docker rm plone6-backend
```

## Próximos pasos

Conozca las [Imágenes oficiales](https://6.docs.plone.org/install/containers/images/index.html) mantenidas por la Comunidad Plone.

También puede ver algunos [ejemplos](https://6.docs.plone.org/install/containers/examples/index.html) de cómo usar dichas imágenes oficiales para arrancar sus proyectos.


