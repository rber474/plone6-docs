---
hide:
  - tags
---

T|Estado|Pendiente|class:pending|
L|:simple-bitbucket:|https://bitbucket.once.es/projects/WEB-PORTAL/repos/biblioteca.tiflologica|
# biblioteca.tiflologica


## Repositorios

| Repositorio                  | URL                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| :simple-subversion: SVN      | [biblioteca.tiflologica](https://evosvn.evonceib.local/svn/webs/empleado/biblioteca.tiflologica)  |
| :simple-bitbucket: Bitbucket | [biblioteca.tiflologica](ssh://git@bitbucket.once.es:7999/web-portal/biblioteca.tiflologica.git)  |

## Descripción

Paquete que contiene los tipos de contenido:
- ``ArchivoFichaTiflologica``
- ``BibliotecaTiflologica``
- ``ExternalResource``
- ``FichaTiflologica``


!!! warning "Atención"

    - Existe una funcionalidad de exportación a MS Word que no podrá ser exportada con la librería embebida.
    - Hay conexión a URL de servicio externo de ``SAGE`` que debe ser autorizado por **host** y por **IP**.
    - Hace uso intensivo de ``ATVocabularyManager`` actualizando los vocabularios con nuevos autores cada vez que se crea o edita una ficha. (Migrar a BBDD)


## Migración

Este paquete contiene personalizaciones complejas.
Hace uso de ``AdvancedQuery`` un paquete de terceros, integrado como libreria en este paquete.

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

No hay workflow específico.


### portal_skins

Contiene múltiples scripts *.py e imágenes.
Personaliza el main_template cuando el contexto sea ``main_template_biblio_tifo``

### Árbol de contenidos
```
biblioteca.tiflologica/
└── trunk
    ├── README.txt
    ├── biblioteca
    │   ├── __init__.py
    │   └── tiflologica
    │       ├── AdvancedQuery
    │       │   ├── AdvancedQuery.py
    │       │   ├── LICENSE.txt
    │       │   ├── README.txt
    │       │   ├── VERSION.txt
    │       │   ├── __init__.py
    │       │   ├── configure.zcml
    │       │   ├── doc
    │       │   │   └── AdvancedQuery.html
    │       │   ├── eval.py
    │       │   ├── ranking.py
    │       │   ├── sorting.py
    │       │   └── tests
    │       │       ├── TestBase.py
    │       │       ├── __init__.py
    │       │       ├── framework.py
    │       │       └── testAdvancedQuery.py
    │       ├── __init__.py
    │       ├── browser
    │       │   ├── __init__.py
    │       │   ├── buscadorview.py
    │       │   ├── carritoTiflo.py
    │       │   ├── configure.zcml
    │       │   ├── controlpanel
    │       │   │   ├── __init__.py
    │       │   │   └── controlpanel.py
    │       │   ├── envioPedido.py
    │       │   ├── estadisticasTiflo.py
    │       │   ├── fichatiflologicaview.py
    │       │   ├── formularioEnvio.py
    │       │   ├── getSubareasView.py
    │       │   ├── listadoExportarAdminView.py
    │       │   ├── listadoExportarView.py
    │       │   ├── mantenimientoView.py
    │       │   ├── panelcontrolView.py
    │       │   ├── resources
    │       │   │   ├── buscador.js
    │       │   │   ├── carritoBiblioTiflo.js
    │       │   │   ├── fichaEdicion.js
    │       │   │   └── formularioEstadisticasTiflo.js
    │       │   ├── resultadoExportarAdminView.py
    │       │   ├── resultadosBuscadorTiflologicoView.py
    │       │   └── templates
    │       │       ├── buscadorview.pt
    │       │       ├── carritoTifloView.pt
    │       │       ├── controlpanel_layout.pt
    │       │       ├── envioPedidoView.pt
    │       │       ├── estadisticasTiflo.pt
    │       │       ├── fichatiflologicauserview.pt
    │       │       ├── fichatiflologicaview.pt
    │       │       ├── formularioEnvioView.pt
    │       │       ├── listadoExportarAdminView.pt
    │       │       ├── listadoExportarUserView.pt
    │       │       ├── mantenimientoview.pt
    │       │       ├── resultadoEstadisticasTiflo.pt
    │       │       ├── resultadoExportarAdminView.pt
    │       │       └── resultadosBuscadorTiflologicoView.pt
    │       ├── catalog.py
    │       ├── config.py
    │       ├── configure.zcml
    │       ├── content
    │       │   ├── ArchivoFichaTiflologica.py
    │       │   ├── BibliotecaTiflologica.py
    │       │   ├── ExternalResource.py
    │       │   ├── FichaTiflologica.py
    │       │   ├── __init__.py
    │       │   └── configure.zcml
    │       ├── docx
    │       │   ├── __init__.py
    │       │   ├── api.py
    │       │   ├── blkcntnr.py
    │       │   ├── compat.py
    │       │   ├── configure.zcml
    │       │   ├── dml
    │       │   │   ├── __init__.py
    │       │   │   ├── color.py
    │       │   │   └── configure.zcml
    │       │   ├── document.py
    │       │   ├── enum
    │       │   │   ├── __init__.py
    │       │   │   ├── base.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── dml.py
    │       │   │   ├── section.py
    │       │   │   ├── shape.py
    │       │   │   ├── style.py
    │       │   │   ├── table.py
    │       │   │   └── text.py
    │       │   ├── exceptions.py
    │       │   ├── image
    │       │   │   ├── __init__.py
    │       │   │   ├── bmp.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── constants.py
    │       │   │   ├── exceptions.py
    │       │   │   ├── gif.py
    │       │   │   ├── helpers.py
    │       │   │   ├── image.py
    │       │   │   ├── jpeg.py
    │       │   │   ├── png.py
    │       │   │   └── tiff.py
    │       │   ├── opc
    │       │   │   ├── __init__.py
    │       │   │   ├── compat.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── constants.py
    │       │   │   ├── coreprops.py
    │       │   │   ├── exceptions.py
    │       │   │   ├── oxml.py
    │       │   │   ├── package.py
    │       │   │   ├── packuri.py
    │       │   │   ├── part.py
    │       │   │   ├── parts
    │       │   │   │   ├── __init__.py
    │       │   │   │   ├── configure.zcml
    │       │   │   │   └── coreprops.py
    │       │   │   ├── phys_pkg.py
    │       │   │   ├── pkgreader.py
    │       │   │   ├── pkgwriter.py
    │       │   │   ├── rel.py
    │       │   │   ├── shared.py
    │       │   │   └── spec.py
    │       │   ├── oxml
    │       │   │   ├── __init__.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── coreprops.py
    │       │   │   ├── document.py
    │       │   │   ├── exceptions.py
    │       │   │   ├── ns.py
    │       │   │   ├── numbering.py
    │       │   │   ├── section.py
    │       │   │   ├── shape.py
    │       │   │   ├── shared.py
    │       │   │   ├── simpletypes.py
    │       │   │   ├── styles.py
    │       │   │   ├── table.py
    │       │   │   ├── text
    │       │   │   │   ├── __init__.py
    │       │   │   │   ├── configure.zcml
    │       │   │   │   ├── font.py
    │       │   │   │   ├── hyperlink.py
    │       │   │   │   ├── paragraph.py
    │       │   │   │   ├── parfmt.py
    │       │   │   │   └── run.py
    │       │   │   └── xmlchemy.py
    │       │   ├── package.py
    │       │   ├── parts
    │       │   │   ├── __init__.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── document.py
    │       │   │   ├── image.py
    │       │   │   ├── numbering.py
    │       │   │   └── styles.py
    │       │   ├── section.py
    │       │   ├── shape.py
    │       │   ├── shared.py
    │       │   ├── styles
    │       │   │   ├── __init__.py
    │       │   │   ├── configure.zcml
    │       │   │   ├── latent.py
    │       │   │   ├── style.py
    │       │   │   └── styles.py
    │       │   ├── table.py
    │       │   ├── templates
    │       │   │   ├── configure.zcml
    │       │   │   ├── default-src.docx
    │       │   │   ├── default-styles.xml
    │       │   │   └── default.docx
    │       │   └── text
    │       │       ├── __init__.py
    │       │       ├── configure.zcml
    │       │       ├── font.py
    │       │       ├── hyperlink.py
    │       │       ├── paragraph.py
    │       │       ├── parfmt.py
    │       │       └── run.py
    │       ├── events
    │       │   ├── __init__.py
    │       │   ├── events.zcml
    │       │   ├── handlers.py
    │       │   └── handlers.py_old
    │       ├── interfaces.py
    │       ├── locales
    │       │   ├── README.txt
    │       │   └── es
    │       │       └── LC_MESSAGES
    │       │           ├── plone.mo
    │       │           ├── plone.po
    │       │           ├── portal.theme.mo
    │       │           └── portal.theme.po
    │       ├── profiles
    │       │   └── default
    │       │       ├── actions.xml
    │       │       ├── biblioteca_tiflologica_default.txt
    │       │       ├── catalog.xml
    │       │       ├── controlpanel.xml
    │       │       ├── cssregistry.xml
    │       │       ├── factorytool.xml
    │       │       ├── jsregistry.xml
    │       │       ├── metadata.xml
    │       │       ├── registry.xml
    │       │       ├── rolemap.xml
    │       │       ├── skins.xml
    │       │       ├── types
    │       │       │   ├── ArchivoFichaTiflologica.xml
    │       │       │   ├── BibliotecaTiflologica.xml
    │       │       │   ├── ExternalResource.xml
    │       │       │   └── FichaTiflologica.xml
    │       │       ├── types.xml
    │       │       └── workflows.xml
    │       ├── setuphandlers.py
    │       ├── skins
    │       │   ├── bibliotecaTiflologica_images
    │       │   │   ├── BJIV-.jpg
    │       │   │   ├── EDM-.jpg
    │       │   │   ├── IJOM-.jpg
    │       │   │   ├── INT-.jpg
    │       │   │   ├── JVIB_.jpg
    │       │   │   ├── altocontraste
    │       │   │   │   ├── gestiona.png
    │       │   │   │   ├── herramientas.gif
    │       │   │   │   ├── herramientas.png
    │       │   │   │   ├── ico_dicc.png
    │       │   │   │   ├── ico_mi-carrito.png
    │       │   │   │   ├── ico_zonapers.png
    │       │   │   │   ├── mail.gif
    │       │   │   │   ├── mail.png
    │       │   │   │   ├── nav_fnd.jpg
    │       │   │   │   ├── search.gif
    │       │   │   │   └── tablon.png
    │       │   │   ├── articulos-capitulos.jpg
    │       │   │   ├── audiovisual.jpg
    │       │   │   ├── biblioteca_folder_icon.png
    │       │   │   ├── bra.png
    │       │   │   ├── ficha_tiflo_icon.png
    │       │   │   ├── help_icon.png
    │       │   │   ├── ico_int_lupa.png
    │       │   │   ├── libro.jpg
    │       │   │   ├── otros.jpg
    │       │   │   ├── pdf.png
    │       │   │   └── site-actions
    │       │   │       ├── herramientas.gif
    │       │   │       ├── herramientas.png
    │       │   │       ├── ico_mi-carrito.png
    │       │   │       ├── ico_zonapers.png
    │       │   │       ├── mail.gif
    │       │   │       └── mail.png
    │       │   ├── bibliotecaTiflologica_scripts
    │       │   │   ├── CONTENT.txt
    │       │   │   ├── actualizarBibliotecaTiflologica.py
    │       │   │   ├── actualizarDatosCompraBT.py
    │       │   │   ├── actualizarFichas.py
    │       │   │   ├── actualizarGruposEspeciales.py
    │       │   │   ├── actualizarGruposFichas.py
    │       │   │   ├── actualizarPermisosArchivos.py
    │       │   │   ├── actualizarPermisosFichas.py
    │       │   │   ├── consultar_V_LOGUNI_NIF.zsql
    │       │   │   ├── creacionUsuarios.py
    │       │   │   ├── diccionario_estadisticas_tiflo.py
    │       │   │   ├── importarArchivosTiflo1.py
    │       │   │   ├── importarArchivosTiflo2.py
    │       │   │   ├── importarArchivosTiflo3.py
    │       │   │   ├── importarArchivosTiflo4.py
    │       │   │   ├── importarArchivosTiflo5.py
    │       │   │   ├── importarArchivosTifloCAT.py
    │       │   │   ├── importarArchivosTifloCAT2.py
    │       │   │   ├── importarDatosTiflo.py
    │       │   │   └── publicarFichas.py
    │       │   ├── bibliotecaTiflologica_styles
    │       │   │   ├── CONTENT.txt
    │       │   │   ├── biblioteca-tiflologica-mobile.css
    │       │   │   └── biblioteca-tiflologica.css
    │       │   └── bibliotecaTiflologica_templates
    │       │       └── main_template_biblio_tiflo.pt
    │       ├── skins.zcml
    │       ├── tests.py
    │       ├── tifloTool.py
    │       ├── tool.gif
    │       └── utiles.py
    ├── biblioteca.tiflologica.egg-info
    │   ├── PKG-INFO
    │   ├── SOURCES.txt
    │   ├── dependency_links.txt
    │   ├── entry_points.txt
    │   ├── namespace_packages.txt
    │   ├── not-zip-safe
    │   ├── paster_plugins.txt
    │   ├── requires.txt
    │   └── top_level.txt
    ├── docs
    │   ├── Archivos
    │   ├── HISTORY.txt
    │   ├── INSTALL.txt
    │   ├── LICENSE.GPL
    │   └── LICENSE.txt
    ├── setup.cfg
    ├── setup.py
    └── six-1.10.0-py2.7.egg
        ├── EGG-INFO
        │   ├── PKG-INFO
        │   ├── SOURCES.txt
        │   ├── dependency_links.txt
        │   ├── not-zip-safe
        │   └── top_level.txt
        └── six.py
``` 
