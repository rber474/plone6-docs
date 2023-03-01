---
myst:
  html_meta:
    "description": "Manage add-ons, packages, and processes"
    "property=og:description": "Manage add-ons, packages, and processes"
    "property=og:title": "Manage add-ons, packages, and processes"
    "keywords": "Plone 6, manage, backend, add-ons, packages, processes, cookiecutter, Zope"
---

# Gestionar complementos y paquetes

Es capítulo asume que Usted ha seguido las instrucciones de [Instalar usando los paquetes](/install/paquetes).

En esta sección, analizamos los detalles del proceso de instalación para que pueda personalizar su instalación de Plone.
También cubre las tareas de administración de rutina que un desarrollador podría realizar.


## Detalles de instalación con Cookiecutter

`Cookiecutter` crea proyectos usando plantillas de proyectos.
Cookiecutter [`cookiecutter-plone-starter`](https://github.com/collective/cookiecutter-plone-starter/) crea un proyecto Plone que puede instalar usando el comando `Make`.
Genera archivos para instalar y configurar tanto el frontend como el backend.
Para el backend, utiliza la plantilla [`cookiecutter-zope-instance`](https://github.com/plone/cookiecutter-zope-instance) para generar los archivos de configuración de una instancia Zope WSGI.


## Configuración con `cookiecutter-zope-instance`

Puede configurar las opciones de su instancia, incluidas las siguientes.

- almacenamiento persistente: blobs, almacenamiento directo de archivos, base de datos relacional, ZEO, etc.
- puertos
- hilos
- caché
- depuración y creación de perfiles para el desarrollo

!!! success "Ver también..."
    Para obtener la lista completa de características, modo de uso y opciones lea [`cookiecutter-zope-instance`'s `README.rst`](https://github.com/plone/cookiecutter-zope-instance#readme).


## Gestionar los paquetes del backend de Plone con `mxdev`

Esta sección describe cómo administrar paquetes para el backend de Plone con `mxdev`.

Para desarrollar complementos para la interfaz de Plone, Volto, consulte `volto/addons/index`.


### El problema con  `pip`

Si desea verificar un paquete central de Plone para desarrollo, o desea anular las restricciones de Plone, normalmente definiría las restricciones con un archivo `constraints.txt` para decirle a `pip` que instale una versión diferente de un Paquete Plone.


``` title="constraints.txt"
# constraints.txt with unresolvable version conflict
-c https://dist.plone.org/release/{PLONE_BACKEND_PATCH_VERSION}/constraints.txt
plone.api>=2.0.0a3
```

Desafortunadamente, `pip` no permite anular las restricciones de esta manera.
`mxdev` resuelve este problema.


### ¡`mxdev` al rescate!

`mxdev` resuelve las restricciones de Plone de acuerdo con sus necesidades de fijación de versiones o verificación de fuentes.
Lee su archivo de configuración `mx.ini` y sus archivos `requirements.txt` y `constraints.txt`.
Luego obtiene los requisitos y restricciones de Plone.
Finalmente, escribe nuevos requisitos combinados en el archivo `requirements-mxdev.txt` y nuevas restricciones en `constraints-mxdev.txt`.
Juntos, estos dos archivos contienen los requisitos y restricciones combinados, pero modificados según la configuración en `mx.ini`.
Los archivos generados indican de dónde se obtuvieron las restricciones y se agregan comentarios cuando fue necesaria una modificación.

`mxdev` no ejecuta `pip` ni instala paquetes.
Debe realizar ese paso.


### Descripción general del uso de `mxdev`

El conjunto predeterminado de archivos para `mxdev` se muestra a continuación.
Están ubicados en el directorio `backend` de su proyecto.

```ini title="requirements.txt"
-c constraints.txt
-e src/project_title

zope.testrunner

# Add required add-ons
# collective.easyform
```


```ini title="constraints.txt"
-c https://dist.plone.org/release/{PLONE_BACKEND_PATCH_VERSION}/constraints.txt
```

```ini title="mx.ini"
; This is a mxdev configuration file
; it can be used to override versions of packages already defined in the
; constraints files and to add new packages from VCS like git.
; to learn more about mxdev visit https://pypi.org/project/mxdev/

[settings]
; example how to override a package version
; version-overrides =
;     example.package==2.1.0a2

; example section to use packages from git
; [example.contenttype]
; url = https://github.com/collective/example.contenttype.git
; pushurl = git@github.com:collective/example.contenttype.git
; extras = test
; branch = feature-7
```

Puede editar estos tres archivos en su proyecto según lo necesite.
A continuación, puede generar archivos de restricciones y requisitos de paquetes y, a continuación, instalar esos paquetes con un solo comando.

```shell
make build-backend
```

`make build-backend` invoca a `mxdev`, que genera los archivos `requirements-mxdev.txt` y `constraints-mxdev.txt`.
Luego invoca `pip` para instalar paquetes con el nuevo archivo de requisitos.

Para recargar los paquetes, detenga su instancia de Zope/sitio de Plone con {kbd}`ctrl-c` e inícielo con el siguiente comando.
```shell
make start-backend
```

!!!success "Ver también..."
    Consulte la [documentación de `mxdev` en su README.md](https://github.com/mxstack/mxdev/blob/main/README.md) para obtener la información completa.


## Tareas de gestión comunes

Esta sección proporciona ejemplos de tareas comunes de administración de paquetes y complementos.
Para los ejemplos, modificaremos los archivos predeterminados de la sección anterior.
También usaremos la interfaz de usuario clásica para la interfaz porque algunos paquetes y complementos aún no se han actualizado para que funcionen con la nueva interfaz.


### Añadir un complemento

Añade una línea con el nombre de tu complemento en el archivo `requirements.txt`.
Este ejemplo utiliza [`collective.easyform`](https://pypi.org/project/collective.easyform/).

``` title="requirements.txt"
collective.easyform
```

Añádelo al archivo `instance.yaml` para que ZOPE sepa que este complemento debería ser cargado:

``` yaml title="instance.yaml" hl_lines="3-6"
default_context:
    load_zcml:
        package_includes: [
            'project_title',
            'collective.easyform',
        ]
```

Detenga el backend con ++ctrl+c++.
Aplique los cambios y reinicie el backend.
No necesita parar el frontend.

```shell
make build-backend
make start-backend
```

En su navegador web, y suponiendo que actualmente haya iniciado sesión como `admin`, visite la URL http://localhost:8080/Plone/prefs_install_products_form.

Luego haga clic en el botón `Instalar` junto a su complemento para completar la instalación del complemento.

Algunos complementos tienen opciones de configuración.
Para configurar tales complementos, regrese al panel de control `Configuración del sitio`.
En la parte inferior de la página, debería ver el encabezado `Configuración del complemento` y un panel de control para configurar el complemento que acaba de instalar.


### Fijar la versión de un complemento

Fije la versión en el archivo `constraints.txt`.

``` title="constraints.txt"
collective.easyform==3.1.0
```

Añada la versión al archivo `requirements.txt`:

``` title="requirements.txt"
collective.easyform
```

Añádalo al archivo `instance.yaml` para que ZOPE sepa que este complemento debería ser cargado:


```yaml title="instance.yaml"
default_context:
    load_zcml:
        package_includes: [
            'project_title',
            'collective.easyform',
        ]
```

Detenga el backend con ++ctrl+c++.
Aplique los cambios y reinicie el backend.

```shell
make build-backend
make start-backend
```

En su navegador web, y suponiendo que actualmente haya iniciado sesión como `admin`, visite la URL http://localhost:8080/Plone/prefs_install_products_form.
Es posible que se deba realizar un paso de actualización en el panel de control de Plone.
Siga la información de actualización, si está presente.
De lo contrario, haga clic en el botón `Instalar` para completar la instalación del complemento.


### Obtener un complemento (Checkout)

Añada el complemento al archivo `requirements.txt`:

``` title="requirements.txt"
collective.easyform
```

En el archivo `mx.ini`, especifique la información para realizar la obtención del complemento:

```ini title="mx.ini"
[collective.easyform]
url=git@github.com:collective/collective.easyform.git
branch=dev-branch-name
extras=test
```

Añádelo al archivo `instance.yaml` para que ZOPE sepa que este complemento debería ser cargado:

```yaml title="instance.yaml"
default_context:
    load_zcml:
        package_includes: [
            'project_title',
            'collective.easyform',
        ]
```

Detenga el backend con ++ctrl+c++.
Aplique los cambios y reinicie el backend.

```shell
make build-backend
make start-backend
```

En su navegador web, y suponiendo que actualmente haya iniciado sesión como `admin`, visite la URL http://localhost:8080/Plone/prefs_install_products_form.
Es posible que se deba realizar un paso de actualización en el panel de control de Plone.
Siga la información de actualización, si está presente.
De lo contrario, haga clic en el botón `Instalar` para completar la instalación del complemento.


### Fijar la versión de un paquete de Plone contra restricciones

Una versión **no** se puede anclar en `constraints.txt` cuando el paquete se menciona en las restricciones de Plone.
Cualquier otra versión del paquete podría fijarse en `constraints.txt`.
 
Fija la versión de un paquete de Plone en el archivo `mx.ini`:

```ini title="mx.ini"
[settings]
# constraints of Plone packages
version-overrides =
    plone.api>=2.0.0a3
```

Guarde los cambios y reinicie el backend

```shell
make build-backend
make start-backend
```

En su navegador web, y suponiendo que actualmente haya iniciado sesión como `admin`, visite la URL http://localhost:8080/Plone/prefs_install_products_form.
Es posible que se deba realizar un paso de actualización en el panel de control de Plone.
Siga la información de actualización, si está presente.


### Descargar un paquete de Plone

Esta sección cubre cómo obtener un paquete de  Core de Plone para desarrollo.

Agregue el paquete de Plone que desea verificar en el archivo `mx.ini`.

```ini title="mx.ini"
[plone.restapi]
url = git@github.com:plone/plone.restapi.git
branch = master
extras = test
```

Detenga el backend con ++ctrl+c++.
Aplique los cambios y reinicie el backend.

```shell
make build-backend
make start-backend
```

En su navegador web, y suponiendo que actualmente haya iniciado sesión como `admin`, visite la URL http://localhost:8080/Plone/prefs_install_products_form.
Es posible que se deba realizar un paso de actualización en el panel de control de Plone.
Siga la información de actualización, si está presente.


### Generar y arrancar su instancia

Cada vez que realice cambios en la configuración de su back-end, por ejemplo cuando instale un complemento o anule un paquete principal de Plone, se necesita una compilación y un reinicio.
Primeramente, detenga su instancia zope/plone ++ctrl+c++.

Ahora genere y arranque la instancia

```shell
make build-backend
make start-backend
```

En un navegador web, visite http://localhost:8080/ para ver que Plone se está ejecutando.

Su instancia se ejecuta en primer plano.


## Detalles de instalación del backend

El `Makefile` en la raíz de su proyecto invoca comandos en `backend/Makefile`.
Aquí hay extractos de `backend/Makefile` para mostrar detalles del comando `make build-backend`.

```makefile title="Makefile"
bin/pip:
	@echo "$(GREEN)==> Setup Virtual Env$(RESET)"
	python3 -m venv .
	bin/pip install -U "pip" "wheel" "cookiecutter" "mxdev"

instance/etc/zope.ini:	bin/pip
	@echo "$(GREEN)==> Install Plone and create instance$(RESET)"
	bin/cookiecutter -f --no-input --config-file instance.yaml https://github.com/plone/cookiecutter-zope-instance
	mkdir -p var/{filestorage,blobstorage,cache,log}

# ...

.PHONY: build-dev
build-dev: instance/etc/zope.ini ## pip install Plone packages
	@echo "$(GREEN)==> Setup Build$(RESET)"
	bin/mxdev -c mx.ini
	bin/pip install -r requirements-mxdev.txt
```


El comando `make build-backend`:

- Invoca el objetivo `build-dev` en `backend/Makefile`.
- Esto invoca el destino `instance/etc/zope.ini`.
- Esto invoca el destino `bin/pip`.

    - Esto crea un entorno virtual `Python` si no existe uno.
    - Instala y actualiza las herramientas de administración de paquetes de Python en ese entorno virtual.

- Volviendo al objetivo `instance/etc/zope.ini`:

    - Esto crea o actualiza la configuración de Zope desde su archivo `instance.yaml` usando `cookiecutter-zope-instance`.
    - Crea directorios especificados, si no existen.

- Volviendo al objetivo `build-dev`:

    - Esto genera los archivos `mxdev` como se describe anteriormente.
    - Instala los paquetes principales de Plone y complementos de los archivos generados por `mxdev`.

Puede configurar su instancia de Zope como se describe en la sección [Tareas de gestión comunes](#tareas-de-gestion-comunes).