# Tema ONCE THEME

El tema principal para Plone 6 de ONCE será ``once.plone6theme``

## Dependencias
Para poder personalizar y compilar los temas basados en DIAZO y el Barceloneta Theme es necesario tener instaladas algunas dependencias:

- **nvm**
- **Node**
- **Yeoman**
- **Yarn**

Puedes consultar cómo realizar la instalación y las versiones necesarias en [Pre-requisitos para la instalación](../install/paquetes/index.md#pre-requisitos-para-la-instalacion)

## Obtener el paquete once.plone6theme

El paquete del tema principal para Plone 6 está ubicado en el repositorio ``WEB-COMMONS/once.plone6theme``

Dentro del archivo ``instance-develop.cfg`` añadiremos el paquete tanto en la sección ``SOURCES`` como en la sección ``develop`` ``eggs``

``` cfg title="instance-develop.cfg"
[sources]
once.plone6theme = git bitbucket.once.es:web-commons/once.plone6theme.git branch=master

[develop]
# development tools
parts =
    vscode

eggs =
    plone.reload
    Products.PDBDebugMode
    plone.app.debugtoolbar
    Products.PrintingMailHost
    pdbpp
    z3c.saconfig
    SQLAlchemy
    once.plone6theme

[versions]
sqlalchemy = 1.4.46

```

Después de ejecutar el comando ``buildout`` el producto aparecéra como instalable en el **panel de control "Complementos"**:

``` shell
bin/buildout
```

Procede a su instalación.


## Servir, compilar y distribuir el tema

### Comandos básicos para el tema

En el propio paquete del tema, en el archivo ``package.json``, están definidos los scripts necesarios para compilar y generar la distribución de este tema:

``` json title="package.json"
{
  "//": "Put here only theme dependencies, devDependencies should stay outside of the theme folder in the package root.",
  "name": "once-theme",
  "version": "1.0.0",
  "license": "MIT",
  "devDependencies": {
    "autoprefixer": "^10.4.8",
    "clean-css-cli": "^5.6.1",
    "nodemon": "^2.0.19",
    "npm-run-all": "^4.1.5",
    "postcss": "^8.4.16",
    "postcss-cli": "^9.0.1",
    "sass": "^1.54.7",
    "stylelint-config-twbs-bootstrap": "^5.0.0",
    "tinymce": "^5.10.2"
  },
  "scripts": {
    "watch": "nodemon --watch styles/ --ext scss --exec \"npm run css-main\"",
    "build": "npm-run-all css-compile-main css-prefix-main css-minify-main",
    "css-main": "npm-run-all css-compile-main css-prefix-main css-minify-main",
    "css-compile-main": "sass --load-path=node_modules --style expanded --source-map --embed-sources --no-error-css styles/theme.scss:styles/theme.css",
    "css-prefix-main": "postcss --config postcss.config.js --replace \"styles/*.css\" \"!styles/*.min.css\"",
    "css-minify-main": "cleancss -O1 --format breakWith=lf --with-rebase --source-map --source-map-inline-sources --output styles/theme.min.css styles/theme.css",
    "css-lint": "stylelint \"styles/**/*.scss\" --cache --cache-location .cache/.stylelintcache"
  },
  "dependencies": {
    "@plone/plonetheme-barceloneta-base": "^3.0.0b6"
  }
}

```

### Paso 1. Instalar las dependencias del tema

Ejecute lo siguiente para instalar las dependencias con ``nvm`` y ``yarn``:

``` shell
cd ~/proyectos/weppor/src/once.plone6theme/src/once/plone6theme/theme
yarn install
```

Esto creará un directorio ``node-modules`` con todas las dependencias necesarias. No te preocupes, este directorio está excluido del control de versiones ``.gitignore``

### Paso 2. Activar el modo compilación online

Para ir compilando los archivos ``.scss`` y ``javascripts`` mientras se modifican los fuentes, puedes ejecutar:

``` shell
npm run watch
```

``` output

> once-theme@1.0.0 watch
> nodemon --watch styles/ --ext scss --exec "npm run css-main"

[nodemon] 2.0.21
[nodemon] to restart at any time, enter `rs`
[nodemon] watching path(s): styles/**/*
[nodemon] watching extensions: scss
[nodemon] starting `npm run css-main`

> once-theme@1.0.0 css-main
> npm-run-all css-compile-main css-prefix-main css-minify-main


> once-theme@1.0.0 css-compile-main
> sass --load-path=node_modules --style expanded --source-map --embed-sources --no-error-css styles/theme.scss:styles/theme.css


> once-theme@1.0.0 css-prefix-main
> postcss --config postcss.config.js --replace "styles/*.css" "!styles/*.min.css"


> once-theme@1.0.0 css-minify-main
> cleancss -O1 --format breakWith=lf --with-rebase --source-map --source-map-inline-sources --output styles/theme.min.css styles/theme.css

[nodemon] clean exit - waiting for changes before restart
```

### Paso 3. Compilación definitiva
Para compilar de manera definitiva y así distribuir los cambios:

``` shell
npm run build
```

``` output
> once-theme@1.0.0 build
> npm-run-all css-compile-main css-prefix-main css-minify-main


> once-theme@1.0.0 css-compile-main
> sass --load-path=node_modules --style expanded --source-map --embed-sources --no-error-css styles/theme.scss:styles/theme.css


> once-theme@1.0.0 css-prefix-main
> postcss --config postcss.config.js --replace "styles/*.css" "!styles/*.min.css"


> once-theme@1.0.0 css-minify-main
> cleancss -O1 --format breakWith=lf --with-rebase --source-map --source-map-inline-sources --output styles/theme.min.css styles/theme.css
```