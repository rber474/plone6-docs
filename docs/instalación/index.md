# Instalación

En esta parte de la documentación, puede encontrar cómo probar Plone y cómo elegir un método de instalación si desea desarrollar en Plone.

## Empezando

!!! example "Qué desea hacer"

    === ":octicons-browser-24: Pruebe una demo"

        Elija una versión

        * [Plone 6 con frontend Volto](https://demo.plone.org/)
        * [Plone 6 Classic UI](https://classic.demo.plone.org/en)
        * [Plone 5.2.x (stable) con Barceloneta Theme](https://52.demo.plone.org/)

    === ":octicons-download-24: Instalar"

        Los desarrolladores pueden elegir entre instalar Plone desde las imagen oficiales de contenedores o a través de los paquetes.

        !!! info ""

            Ayúdame a [Elegir un método de instalación](#eleccion-de-un-metodo-de-instalacion)


## Elección de un método de instalación

### Contenedores

Las imágenes del contenedor de Plone 6 cumplen con la [Open Container Initiative (OCI)](https://opencontainers.org/). Deben funcionar con cualquier motor de contenedor compatible con OCI para desarrollar, administrar y ejecutar imágenes de Plone 6. Dos opciones populares incluyen [podman](https://podman.io/) y [Docker](https://www.docker.com/products/docker-desktop/).

Las imágenes de Plone 6 tienen todos los requisitos del sistema, requisitos previos y Plone 6 ya instalado, excepto los requisitos necesarios para ejecutar el motor del contenedor.

Esta opción es el método más rápido para instalar y desarrollar Plone 6 y sus paquetes.



[:fontawesome-brands-docker: Usar contenedores para instalar Plone](contenedores/index.md){ .md-button .md-button--primary }

### Paquetes

Podría haber algunos casos donde usar la imagen Plone 6 y los contenedores no sea práctico o no esté recomendado para usted, ya que:

* Utiliza una BBDD SQL que no sea PostgreSQL.
* Desarrolla aplicaciones personalizadas, temas y complementos para Plone.
* Utiliza un flujo de trabajo para despliegues que tiene requisitos específicos.

Para estas situaciones, Plone 6 puede ser instalado a través de sus paquetes.

Puede ser un desafío si se topa con los requisitos del sistema o necesita resolver conflictos entre los paquetes requeridos.

Este método lleva más tiempo que usar contenedores.

[:octicons-package-24: Instalar Plone usando los paquetes](paquetes/index.md){ .md-button .md-button--primary }

## Requisitos del sistema

Los requisitos del sistema dependerán del método de instalación seleccionado.

* [Requisitos del sistema para Contenedores](contenedores/index.md#requisitos-de-sistema)
* [Requisitos del sistema en la instalación por paquetes](paquetes/index.md#requisitos-de-sistema)
