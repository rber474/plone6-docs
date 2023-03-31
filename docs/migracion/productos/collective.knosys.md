---
hide:
  - tags
---

T|Estado|Pendiente|class:pending|
L|:simple-bitbucket:|https://bitbucket.once.es/projects/WEB-COMMONS/repos/collective.knosys|
# collective.knosys


## Repositorios

| Repositorio                  | URL                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| :simple-subversion: SVN      | [collective.knosys](https://evosvn.evonceib.local/svn/webs/commons/collective.knosys)  |
| :simple-bitbucket: Bitbucket | [collective.knosys](ssh://git@bitbucket.once.es:7999/web-commons/collective.knosys.git)  |

## Descripción

Paquete sencillo para la configuración base de los sitios de bibliotecas.

## Migración

Este paquete no contiene ninguna personalización compleja.

### Tipos de contenido

Ninguno

### BibliotecaSiteTool

Migración a HelperView

### Workflow definitions

#### portal_workflow_vocabulario

Asignado a los tipos de contenidos:

- TreeVocabularyTerm
- SimpleVocabularyTerm

### portal_skins

- Contiene ``zsql methods`` que ya no se usan en ninguno de los tres paquetes de bibliotecas.
- ``logout.cpy``: Personaliza el validator python script


### Árbol de contenidos
```
collective.knosys/
├── README.txt
├── biblioteca
│   ├── __init__.py
│   └── site
│       ├── __init__.py
│       ├── bibliotecaSiteTool.py
│       ├── config.py
│       ├── configure.zcml
│       ├── profiles
│       │   ├── default
│       │   │   ├── bibliotecaSite_maker.txt
│       │   │   ├── import_steps.xml
│       │   │   ├── metadata.xml
│       │   │   ├── skins.xml
│       │   │   ├── workflows
│       │   │   │   └── portal_workflow_vocabulario
│       │   │   │       └── definition.xml
│       │   │   └── workflows.xml
│       │   └── uninstall
│       │       └── biblioteca_site-uninstall.txt
│       ├── setuphandlers.py
│       ├── skins
│       │   ├── bibliotecaSite_scripts
│       │   │   ├── insertarAccesoBiblioteca.zsql
│       │   │   ├── logout.cpy
│       │   │   ├── obtenerAccesoTerminologiaTotales.zsql
│       │   │   ├── obtenerAccesoTerminologiaUnicos.zsql
│       │   │   ├── obtenerAccesoTifloTotales.zsql
│       │   │   ├── obtenerAccesoTifloUnicos.zsql
│       │   │   └── registrarAccesoBiblioteca_OBSOLETO.py
│       │   └── bibliotecaSite_templates
│       │       └── main_template_OBSOLETO.pt
│       ├── skins.zcml
│       ├── tests.py
│       └── tool.gif
├── dist
│   └── collective.knosys-1.17-py2.7.egg
├── docs
│   ├── HISTORY.txt
│   ├── INSTALL.txt
│   ├── LICENSE.GPL
│   └── LICENSE.txt
├── setup.cfg
└── setup.py
``` 