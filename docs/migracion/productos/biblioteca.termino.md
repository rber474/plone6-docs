---
hide:
  - tags
---

T|Estado|Pendiente|class:pending|
L|:simple-bitbucket:|https://bitbucket.once.es/projects/WEB-PORTAL/repos/biblioteca.termino|
# biblioteca.termino


## Repositorios

| Repositorio                  | URL                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| :simple-subversion: SVN      | [biblioteca.termino](https://evosvn.evonceib.local/svn/webs/empleado/biblioteca.termino)  |
| :simple-bitbucket: Bitbucket | [biblioteca.termino](ssh://git@bitbucket.once.es:7999/web-portal/biblioteca.termino.git)  |

## Descripción

Paquete que contiene los tipos de contenido ``termino`` y ``contenedorTermino``

!!! warning "Atención"

    Existe una funcionalidad de exportación a MS Word que no podrá ser exportada con la librería embebida.


## Migración

Este paquete no contiene ninguna personalización compleja.

### Tipos de contenido

- Archetype contenedorTermino
- Archetype Termino

### Eventos

Este paquete tiene eventos registrados

### BibliotecaTerminoTool

Migración a HelperView

### Indices y metadatos personalizados

Se crean índices en el catálogo.

!!! failure "Atención"

    Se define el índice getExtrasZCTextIndex ``'Okapi BM25 Rank'``

### Workflow definitions

#### biblioteca_workflow_contenedor_termino
Asignado a los tipos de contenidos:

- contenedorTermino


#### biblioteca_workflow_termino

Asignado a los tipos de contenidos:

- Termino


### portal_skins

Contiene múltiples scripts *.py, imágenes y estilos css.

### Árbol de contenidos
```
biblioteca.termino/
├── README.txt
├── biblioteca
│   ├── __init__.py
│   └── termino
│       ├── __init__.py
│       ├── bibliotecaTerminoTool.py
│       ├── browser
│       │   ├── __init__.py
│       │   ├── buscadorTerminosView.py
│       │   ├── configure.zcml
│       │   ├── dependenciasView.py
│       │   ├── estadisticasTerminologia.py
│       │   ├── resultadosBuscadorTerminosView.py
│       │   ├── templates
│       │   │   ├── buscador_terminos.pt
│       │   │   ├── buscador_terminos_resultados.pt
│       │   │   ├── dependencias_termino.pt
│       │   │   ├── estadisticasTerminologia.pt
│       │   │   ├── ficha_termino.pt
│       │   │   └── resultadoEstadisticasTerminologia.pt
│       │   └── terminoView.py
│       ├── config.py
│       ├── configure.zcml
│       ├── content
│       │   ├── Termino.py
│       │   ├── __init__.py
│       │   ├── configure.zcml
│       │   └── contenedorTermino.py
│       ├── docx
│       │   ├── __init__.py
│       │   ├── api.py
│       │   ├── blkcntnr.py
│       │   ├── compat.py
│       │   ├── configure.zcml
│       │   ├── dml
│       │   │   ├── __init__.py
│       │   │   ├── color.py
│       │   │   └── configure.zcml
│       │   ├── document.py
│       │   ├── enum
│       │   │   ├── __init__.py
│       │   │   ├── base.py
│       │   │   ├── configure.zcml
│       │   │   ├── dml.py
│       │   │   ├── section.py
│       │   │   ├── shape.py
│       │   │   ├── style.py
│       │   │   ├── table.py
│       │   │   └── text.py
│       │   ├── exceptions.py
│       │   ├── image
│       │   │   ├── __init__.py
│       │   │   ├── bmp.py
│       │   │   ├── configure.zcml
│       │   │   ├── constants.py
│       │   │   ├── exceptions.py
│       │   │   ├── gif.py
│       │   │   ├── helpers.py
│       │   │   ├── image.py
│       │   │   ├── jpeg.py
│       │   │   ├── png.py
│       │   │   └── tiff.py
│       │   ├── opc
│       │   │   ├── __init__.py
│       │   │   ├── compat.py
│       │   │   ├── configure.zcml
│       │   │   ├── constants.py
│       │   │   ├── coreprops.py
│       │   │   ├── exceptions.py
│       │   │   ├── oxml.py
│       │   │   ├── package.py
│       │   │   ├── packuri.py
│       │   │   ├── part.py
│       │   │   ├── parts
│       │   │   │   ├── __init__.py
│       │   │   │   ├── configure.zcml
│       │   │   │   └── coreprops.py
│       │   │   ├── phys_pkg.py
│       │   │   ├── pkgreader.py
│       │   │   ├── pkgwriter.py
│       │   │   ├── rel.py
│       │   │   ├── shared.py
│       │   │   └── spec.py
│       │   ├── oxml
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   ├── coreprops.py
│       │   │   ├── document.py
│       │   │   ├── exceptions.py
│       │   │   ├── ns.py
│       │   │   ├── numbering.py
│       │   │   ├── section.py
│       │   │   ├── shape.py
│       │   │   ├── shared.py
│       │   │   ├── simpletypes.py
│       │   │   ├── styles.py
│       │   │   ├── table.py
│       │   │   ├── text
│       │   │   │   ├── __init__.py
│       │   │   │   ├── configure.zcml
│       │   │   │   ├── font.py
│       │   │   │   ├── hyperlink.py
│       │   │   │   ├── paragraph.py
│       │   │   │   ├── parfmt.py
│       │   │   │   └── run.py
│       │   │   └── xmlchemy.py
│       │   ├── package.py
│       │   ├── parts
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   ├── document.py
│       │   │   ├── image.py
│       │   │   ├── numbering.py
│       │   │   └── styles.py
│       │   ├── section.py
│       │   ├── shape.py
│       │   ├── shared.py
│       │   ├── styles
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   ├── latent.py
│       │   │   ├── style.py
│       │   │   └── styles.py
│       │   ├── table.py
│       │   ├── templates
│       │   │   ├── configure.zcml
│       │   │   ├── default-src.docx
│       │   │   ├── default-styles.xml
│       │   │   └── default.docx
│       │   └── text
│       │       ├── __init__.py
│       │       ├── configure.zcml
│       │       ├── font.py
│       │       ├── hyperlink.py
│       │       ├── paragraph.py
│       │       ├── parfmt.py
│       │       └── run.py
│       ├── events
│       │   ├── __init__.py
│       │   ├── events.zcml
│       │   └── handlers.py
│       ├── html
│       │   ├── __init__.py
│       │   ├── configure.zcml
│       │   ├── converter.py
│       │   ├── dispatcher.py
│       │   └── tag_dispatchers
│       │       ├── __init__.py
│       │       ├── blockquote.py
│       │       ├── code.py
│       │       ├── configure.zcml
│       │       ├── emphasis.py
│       │       ├── heading.py
│       │       ├── linebreak.py
│       │       ├── link.py
│       │       ├── list_item.py
│       │       ├── paragraph.py
│       │       └── strong.py
│       ├── interfaces
│       │   ├── __init__.py
│       │   └── interfaces.py
│       ├── locales
│       │   ├── README.txt
│       │   └── es
│       │       └── LC_MESSAGES
│       │           ├── plone.mo
│       │           ├── plone.po
│       │           ├── portal.theme.mo
│       │           └── portal.theme.po
│       ├── profiles
│       │   ├── default
│       │   │   ├── bibliotecaTermino_maker.txt
│       │   │   ├── cssregistry.xml
│       │   │   ├── factorytool.xml
│       │   │   ├── import_steps.xml
│       │   │   ├── metadata.xml
│       │   │   ├── rolemap.xml
│       │   │   ├── skins.xml
│       │   │   ├── types
│       │   │   │   ├── ContenedorTermino.xml
│       │   │   │   └── Termino.xml
│       │   │   ├── types.xml
│       │   │   ├── workflows
│       │   │   │   ├── biblioteca_workflow_contenedor_termino
│       │   │   │   │   └── definition.xml
│       │   │   │   └── biblioteca_workflow_termino
│       │   │   │       ├── definition.xml
│       │   │   │       └── scripts
│       │   │   │           ├── aprobar_contenido.py
│       │   │   │           └── send_notification.py
│       │   │   └── workflows.xml
│       │   └── uninstall
│       │       └── biblioteca_termino-uninstall.txt
│       ├── setuphandlers.py
│       ├── skins
│       │   ├── bibliotecaTermino_images
│       │   │   ├── termino_folder_icon.png
│       │   │   └── termino_icon.png
│       │   ├── bibliotecaTermino_scripts
│       │   │   ├── actualizarContenedorTermino.py
│       │   │   ├── exportarDependencias.py
│       │   │   ├── importarAccesibilidad.py
│       │   │   ├── importarAccesoInformacion.py
│       │   │   ├── importarArchivosTerminos.py
│       │   │   ├── importarAutonomiaPersonal.py
│       │   │   ├── importarCSVS.py
│       │   │   ├── importarDatosTerminos.py
│       │   │   ├── importarEducacionInclusiva.py
│       │   │   ├── importarProductosApoyo.py
│       │   │   ├── importarRevisores.py
│       │   │   ├── importarSordoceguera.py
│       │   │   ├── importarTerminos.py
│       │   │   └── importarVision.py
│       │   ├── bibliotecaTermino_styles
│       │   │   ├── biblioteca-termino-mobile.css
│       │   │   └── biblioteca-termino.css
│       │   └── bibliotecaTermino_templates
│       │       ├── termino_folder_icon.png
│       │       └── termino_icon.png
│       ├── skins.zcml
│       ├── tests.py
│       └── tool.gif
├── dist
│   └── biblioteca.termino-1.21-py2.7.egg
├── docs
│   ├── HISTORY.txt
│   ├── INSTALL.txt
│   ├── LICENSE.GPL
│   └── LICENSE.txt
├── setup.cfg
└── setup.py
``` 