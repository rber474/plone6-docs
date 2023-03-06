# Instalar Plone 6 con buildout

## Configuración por defecto de proyectos buildout

!!! info "Información"

    Puedes obviar este paso si ya dispones de una configuración ``~/.buildout/default.cfg``

Podemos agregar las configuraciones genéricas para todos nuestros proyectos Buildout, para esto ejecutamos los siguientes comando:

``` shell
mkdir -p ~/.buildout/{downloads,eggs,extends,zope}
nano ~/.buildout/default.cfg
```

Y añadiremos lo siguiente al archivo ``~/.buildout/default.cfg``:

``` cfg title="~/.buildout/default.cfg"
[buildout]
download-cache = /ruta/absoluta/al/home/del/usuario/.buildout/download-cache
eggs-directory = /ruta/absoluta/al/home/del/usuario/.buildout/eggs
extends-cache = /ruta/absoluta/al/home/del/usuario/.buildout/extends
zope-directory = /ruta/absoluta/al/home/del/usuario/.buildout/zope
abi-tag-eggs = true
```

Gracias a esto, mantendremos una caché de los productos descargados, facilitando futuras instalaciones.

## Preparación de nuestro entorno de trabajo

### Creación de la carpeta de proyecto ``weppor``

Crearemos ahora la carpeta donde instalaremos nuestra instancia Plone 6

``` shell
mkdir -p ~/proyectos/weppor && cd $_
```

## Instalación de Plone 6
### Creación del entorno virtual python 3.11.2
Hay que crear un entorno virtual de Python dentro de la carpeta del buildout
para aislar completamente buildout de las diferentes librerías instaladas

Nos aseguramos que estamos usando ``python 3.11.2``

``` shell
python3 -V
```
``` output
Python 3.11.2
```

Creamos el entorno virtual:
!!! warning "Importante"
    El punto final del comando es necesario para crear el entorno correctamente

``` shell
virtualenv -p python3.11.2 .
```

### Descargar el proyecto buildout

Descargaremos la plantilla de buildout de Plone 6 desde el repositorio bitbucket, bien por ``ssh`` bien por ``https``

``` shell title="ssh"
ssh://git@bitbucket.once.es:7999/web-portal/buildout.plone6.git
```

``` shell title="https"
https://bitbucket.once.es/scm/web-portal/buildout.plone6.git
```

!!! danger "Aviso"
    Evita cualquier modificación de los archivos sobre la rama ``main``.

    Es conveniente crear un branch local y realizar allí las modificaciones.
    No se permitirán merges sobre la rama ``main``, **sólo ``pull requests``**.

    Puedes crear una rama y cambiar a ella con el siguiente comando:
    ``` git
    git checkout -b ＜new-branch＞
    ```

    O bien, puedes crear un *fork* de este repositorio para trabajar en local con tus cambios y después solicitar un ``pull request``

### Instalación de requirements.txt
Para evitar problemas de versiones con setuptools y zc.buildout se fijan las
versiones de ambas librerías tanto en un archivo requirements.txt como en
el propio buildout.

``` shell
./bin/pip install -r requirements.txt
```

### Instalación de entorno de desarrollo
Ya podemos instalar Plone con buildout. Esta receta instalará:

- ``Plone 6.0.2`` en modo ``standalone`` junto con ``Volto``.
- ``varnish`` como proxy caché.
- ``supervisor`` para controlar los servicios.

``` shell
bin/buildout -vv
```

El proceso tardará un buen rato, aprovecha para tomarte un café :simple-coffeescript:.