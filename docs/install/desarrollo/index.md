# Instalación en Ubuntu WSL

## Cómo solucionar 'System has not been booted with systemd' error?
La respuesta simple es no utilizar el comando systemctl. En vez de eso, puedes usar el comando sysvinit equivalente.

No es complicado y ambos comandos tienen síntaxis similar.

Esta tabla debería serte de ayuda.

!!! tip "Equivalencias de comandos entre Systemd y Sysvinit"

    | **Comando Systemd**            | **Comando Sysvinit**         |
    |--------------------------------|------------------------------|
    | systemctl start service_name   | service service_name start   |
    | systemctl stop service_name    | service service_name stop    |
    | systemctl restart service_name | service service_name restart |
    | systemctl status service_name  | service service_name status  |
    | systemctl enable service_name  | chkconfig service_name on    |
    | systemctl disable service_name | chkconfig service_name off   |

## Instalación de las dependencias

### Dependencias del SO

1. Actualizar las dependencias base de Linux

    ``` shell
    sudo apt-get update && sudo apt-get upgrade -y
    ```

    ``` shell
    sudo apt-get install -y make build-essential libssl-dev libxml2-dev libxslt1-dev libbz2-dev \
    libjpeg62-dev libreadline-dev wv poppler-utils wget curl git \
    zlib1g-dev libsqlite3-dev llvm libncurses5-dev libncursesw5-dev \
    xz-utils tk-dev libffi-dev liblzma-dev
    ```

2. Instalar dependencias para NGINX, HAProxy y otros

    ``` shell
    sudo apt-get -y install libxml2-dev \
    libcairo2-dev \
    libglib2.0-dev \
    libgsl-dev \
    libgtk-3-dev \
    libgtksourceview-3.0-dev \
    libpcap-dev \
    python3-docutils \
    libaio1
    ```

### Instalar y activar OpenSSH-Server

- Instalar el servicio ssh

    ``` shell
    sudo apt install openssh-server
    ```

- Activar el servicio ssh

    ``` shell
    sudo service ssh start
    sudo service ssh status
    ```

    ``` output
    * sshd is running
    ```

### Instalar pyenv
pyenv es una herramienta maravillosa para administrar múltiples versiones de Python, ejecutando los siguientes comandos:

``` shell
exec $SHELL
curl -L https://github.com/pyenv/pyenv-installer/raw/master/bin/pyenv-installer | bash

cat >> ~/.bashrc <<'EOF'
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)"
eval "$(pyenv init -)"
EOF

source ~/.bashrc
```

### Instalar Python 3.11.2 con pyenv

Para los desarrollos de Plone 6, se ha seleccionado la versión de python actual ``3.11.2``. Vamos a instalar dicha versión usando pyenv.

``` shell
pyenv install -l | egrep '^\s*3\.11\.'
pyenv install 3.11.2
pyenv versions
python3 -V
pyenv global 3.11.2
python3 -V
```

Para asegurarnos que, en los proyectos de Plone 5, la versión que se usa es Python 3.8.16, vamos a la ruta ``~/proyectos/<nombre-sitio-prueba-plone5>/`` y ejecutamos
``` shell
pyenv local 3.8.16
```
Si bien, **siempre es aconsejable el uso de ``virtualenv``**

### Instalar Oracle InstantClient 21.9.0

A continuación, vamos a descargar e instalar ``Oracle InstantClient`` en Ubuntu. ``Oracle InstantClient`` es un software especial para usuarios Oracle que se utiliza para conectar al servidor Oracle.

#### Descargamos y descomprimimos los archivos

Descargaremos la utilidad ``Unzip``, el software de Oracle y crearemos las ruta de destino:

``` shell
sudo apt-get install unzip
sudo mkdir ~/temp && cd $_
sudo mkdir /opt/oracle
sudo mkdir -p /opt/oracle/instantclient_12_2/network/admin
sudo wget https://download.oracle.com/otn_software/linux/instantclient/219000/instantclient-basic-linux.x64-21.9.0.0.0dbru.zip
sudo wget https://download.oracle.com/otn_software/linux/instantclient/219000/instantclient-sqlplus-linux.x64-21.9.0.0.0dbru.zip
sudo unzip ~/temp/instantclient-basic-linux.x64-21.9.0.0.0dbru.zip -d /opt/oracle
sudo unzip ~/temp/instantclient-sqlplus-linux.x64-21.9.0.0.0dbru.zip -d /opt/oracle
```

#### Configurar SQLPlus

``SQLPlus`` es una utilidad de comandos en línea que puede ser usada para conectar y gestionar bases de datos Oracle. En este paso, configuraremos el path de la librería LD. Añadiremos estas dos líneas a perfil del usuario:

``` shell
nano ~/.profile
```

Recargamos el perfil
``` shell
source ~/.profile
```

Y copiaremos estas líneas al final del archivo:

``` cfg title="~/.profile"
export PATH="$PATH:/opt/oracle/instantclient_21_9"
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/opt/oracle/instantclient_21_9"
```

Y probaremos la conexión:

``` shell
sqlplus dhani@192.168.100.62/pdb
```

``` Output
SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 6 15:04:01 2023
Version 21.9.0.0.0

Copyright (c) 1982, 2022, Oracle.  All rights reserved.

Enter password: 
```
Para salir, presione ++ctrl+c++ y luego ++enter++

### Creación del tnsnames.ora
Ahora crearemos el archivo ``tnsnames.ora``, necesario para configurar el servicio ``DWEB``

``` shell
sudo mkdir -p /opt/oracle/instantclient_12_2/network/admin
sudo nano /opt/oracle/instantclient_12_2/network/admin/tnsnames.ora
```

Y copiaremos el siguiente contenido:

``` cfg title="tnsnames.ora"
DWEB =
(DESCRIPTION =
    (ADDRESS =
        (PROTOCOL = TCP)
        (HOST = $(hostname).local)
        (PORT = 1559)
    )
    (CONNECT_DATA =
        (SERVER = DEDICATED)
        (SID = DWEB)
    )
)
```
Guardamos y salimos: ++ctrl+o++ ++ctrl+x++

Probamos la conexión, **sustituyendo ``<username>`` por el usuario y ``<password>`` por la clave**:

``` shell
sqlplus <username>/<password>@(DESCRIPTION = (ADDRESS = (PROTOCOL = TCP) (HOST = $(hostname).local) (PORT = 1559) ) (CONNECT_DATA = (SERVER = DEDICATED) (SID = DWEB) ) )
```

``` output
SQL*Plus: Release 21.0.0.0.0 - Production on Mon Mar 6 17:31:08 2023
Version 21.9.0.0.0

Copyright (c) 1982, 2022, Oracle.  All rights reserved.

Hora de Ultima Conexion Correcta: Lun Mar 06 2023 17:29:42 +01:00

Conectado a:
Oracle Database 19c Enterprise Edition Release 19.0.0.0.0 - Production
Version 19.10.0.0.0

SQL>
```
