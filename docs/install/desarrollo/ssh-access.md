# Acceso SSH a los repositorios desde el servidor Ubuntu

Para disponer de acceso SSH a los repositorios, por ejemplo a :simple-bitbucket: **Bitbucket** o a :simple-github: **Github**, lo más sencillo es crear un par de claves ssh pública/privada que simplifique el proceso de autenticación/autorización en los repositorios.

## Creación del directorio ``.ssh``

1. Si no disponemos del directorio ``~/.ssh``, lo creamos. La opción **``-p``** nos asegura que el sistema no devolverá un error si el directorio ya existe:

``` shell
mkdir -p ~/.ssh
```

2. Cambiaremos los permisos del directorio, para que el usuario tenga permisos de **lectura**, **escritura** y **ejecución**:

``` shell
chmod 0700 ~/.ssh
```

3. Ejecutaremos el comando ``ssh-keygen`` para crear el par de claves RSA:

``` shell
ssh-keygen
```

4. Durante el proceso se nos preguntará por el path. Pulsa ++enter++ para almacenar la clave en el directorio ``.ssh``
5. El sistema también nos preguntará si se debe añadir una frase de clave como capa de seguridad adicional, puedes dejarla en blanco.

## Añadir la clave pública RSA en tu repositorio

El archivo ``~/.ssh/id_rsa.pub`` contiene la contiene la clave pública que ya puedes subir al repositorio de tu elección.

<figure markdown>
  ![Ejemplo del contenido de id_rsa.pub](../../_static/output-from-cat-ssh-key-pub-pnap.png){ width="100%" }
  <figcaption>id_rsa.pub</figcaption>
</figure>

Sólo tienes que copiar el contenido y crear una clave ssh desde tu cuenta de Github, BitBucket o Gitlab.


## Configurar el acceso al repositorio con ``~/.ssh/config``

Vamos ahora a configurar el acceso al repositorio para que nuestro ``Plone`` pueda descargar los paquetes usando ``git-ssh``
Para ello vamos a crear un archivo ``~/.ssh/config``:

``` shell
nano ~/.ssh/config
```

y vamos a pegar y editar este contenido:

``` cfg title="~/.ssh/config"
Host <dominio-del-repositorio>
    Port <port>
    User <username>
    IdentitiesOnly yes
    IdentityFile ~/.ssh/id_rsa
    StrictHostKeyChecking no
```


## Conclusión

En este capítulo hemos aprendido a crear y configurar el acceso ``.ssh`` para poder obtener los repositorios por ese protocolo.