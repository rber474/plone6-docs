# Instalación en Ubuntu WSL

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
    python-docutils
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