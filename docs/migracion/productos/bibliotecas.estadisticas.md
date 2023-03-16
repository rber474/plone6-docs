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

Paquete de estadísticas para bibliotecas


!!! warning "Atención"

    - Existen múltiples ``ZSQL Methods`` que no están migrados!


## Migración


### Tipos de contenido

Ninguno

### Eventos

Este paquete tiene eventos registrados

### BibliotecaTerminoTool

Migración a HelperView

### Indices y metadatos personalizados

Ninguno

### Workflow definitions

No hay workflow específico.


### portal_skins

Contiene múltiples scripts :simple-python: *.py, scritps :simple-jquery: JQuery y múltiples templates.

### Árbol de contenidos
```
bibliotecas.estadisticas/
├── MANIFEST.in
├── README.txt
├── bibliotecas
│   ├── __init__.py
│   └── estadisticas
│       ├── EstadisticasTool.py
│       ├── README.txt
│       ├── __init__.py
│       ├── browser
│       │   ├── __init__.py
│       │   ├── configure.zcml
│       │   ├── portlets
│       │   │   ├── MasVisitadosPortlet.py
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   └── templates
│       │   │       ├── CargaPortletAjax.pt
│       │   │       └── MasVisitadosPortlet.pt
│       │   ├── visitados_summary_view.pt
│       │   └── visitados_summary_view.py
│       ├── config.py
│       ├── configure.zcml
│       ├── content
│       │   ├── __init__.py
│       │   └── configure.zcml
│       ├── interfaces
│       │   └── __init__.py
│       ├── portlets
│       │   ├── __init__.py
│       │   └── configure.zcml
│       ├── profiles
│       │   └── default
│       │       ├── bibliotecas.estadisticas_various.txt
│       │       ├── factorytool.xml
│       │       ├── jsregistry.xml
│       │       ├── metadata.xml
│       │       ├── portlets.xml
│       │       ├── skins.xml
│       │       └── types.xml
│       ├── setuphandlers.py
│       ├── skins
│       │   ├── estadisticas_scripts
│       │   │   ├── Adaptar_centros.py
│       │   │   ├── Ejecucion_actualizarOficinaEstadisticas.py
│       │   │   ├── Ejecucion_actualizar_in_acces_empleado.zsql
│       │   │   ├── Ejecucion_actualizar_in_acces_ofna.zsql
│       │   │   ├── Ejecucion_consultar_datos_accesos.zsql
│       │   │   ├── Exportar_favorito.py
│       │   │   ├── Exportar_gestiona.py
│       │   │   ├── Exportar_promociones.py
│       │   │   ├── Exportar_sugerencias.py
│       │   │   ├── Exportar_suscripciones.py
│       │   │   ├── Exportar_valoraciones.py
│       │   │   ├── Exportar_zope _old.py
│       │   │   ├── Exportar_zope.py
│       │   │   ├── Exportar_zope_TEST.py
│       │   │   ├── actualizar_datos
│       │   │   │   ├── actualizarGestorEstadisticas.py
│       │   │   │   ├── actualizarOficinaEstadisticas.py
│       │   │   │   ├── actualizar_in_acces_empleado.zsql
│       │   │   │   ├── actualizar_in_acces_ofna.zsql
│       │   │   │   ├── actualizar_in_usu_gest.zsql
│       │   │   │   └── consultar_datos_accesos.zsql
│       │   │   ├── actualizar_registro_acceso_biblio.py
│       │   │   ├── diccionario_estadisticas.py
│       │   │   ├── diccionario_estadisticas_TEST.py
│       │   │   ├── validar_estadisticas.vpy
│       │   │   ├── validar_estadisticas_TEST.vpy
│       │   │   └── validar_estadisticas_gestiona.vpy
│       │   ├── estadisticas_templates
│       │   │   ├── bibliotecas.estadisticas.js
│       │   │   ├── estadistica_buscador.cpt
│       │   │   ├── estadistica_buscador.cpt.metadata
│       │   │   ├── estadistica_buscador_listado.cpt
│       │   │   ├── estadistica_favorito.cpt
│       │   │   ├── estadistica_favorito.cpt.metadata
│       │   │   ├── estadistica_favorito_listado.cpt
│       │   │   ├── estadistica_favorito_listado.cpt.metadata
│       │   │   ├── estadistica_gestiona.cpt
│       │   │   ├── estadistica_gestiona.cpt.metadata
│       │   │   ├── estadistica_gestiona_listado.cpt
│       │   │   ├── estadistica_gestiona_listado.cpt.metadata
│       │   │   ├── estadistica_inscrip_promo.cpt
│       │   │   ├── estadistica_inscrip_promo.cpt.metadata
│       │   │   ├── estadistica_inscrip_promo_listado.cpt
│       │   │   ├── estadistica_inscrip_promo_listado.cpt.metadata
│       │   │   ├── estadistica_paleta_color.cpt
│       │   │   ├── estadistica_paleta_color.cpt.metadata
│       │   │   ├── estadistica_paleta_color_listado.cpt
│       │   │   ├── estadistica_paleta_color_listado.cpt.metadata
│       │   │   ├── estadistica_paleta_color_listado_anterior.cpt
│       │   │   ├── estadistica_sugerencia.cpt
│       │   │   ├── estadistica_sugerencia.cpt.metadata
│       │   │   ├── estadistica_sugerencia_listado.cpt
│       │   │   ├── estadistica_sugerencia_listado.cpt.metadata
│       │   │   ├── estadistica_suscripcion.cpt
│       │   │   ├── estadistica_suscripcion.cpt.metadata
│       │   │   ├── estadistica_suscripcion_listado.cpt
│       │   │   ├── estadistica_suscripcion_listado.cpt.metadata
│       │   │   ├── estadistica_valoracion.cpt
│       │   │   ├── estadistica_valoracion.cpt.metadata
│       │   │   ├── estadistica_valoracion_listado.cpt
│       │   │   ├── estadistica_valoracion_listado.cpt.metadata
│       │   │   ├── estadistica_zope.cpt
│       │   │   ├── estadistica_zope.cpt.metadata
│       │   │   ├── estadistica_zope_TEST.cpt
│       │   │   ├── estadistica_zope_TEST.cpt.metadata
│       │   │   ├── estadistica_zope_listado.cpt
│       │   │   ├── estadistica_zope_listado.cpt.metadata
│       │   │   ├── estadistica_zope_listado_TEST.cpt
│       │   │   ├── estadistica_zope_listado_TEST.cpt.metadata
│       │   │   ├── jquery.datatable.min.js
│       │   │   ├── jquery.tablesorter.min.js
│       │   │   ├── jquery.tablesorter.widgets.min.js
│       │   │   └── principal_estadisticas.pt
│       │   └── estadisticas_zsql
│       │       ├── accesos_unicos_general.zsql
│       │       ├── accesos_unicos_general_TEST.zsql
│       │       ├── consultar_accesos_estadisticas.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_count.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_count_unicos.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_mejorado.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_todos.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_usuario_count.zsql
│       │       ├── consultar_accesos_estadisticas_filtrado_usuario_todos.zsql
│       │       ├── consultar_accesos_estadisticas_listado_catprof.zsql
│       │       ├── consultar_accesos_estadisticas_listado_centro.zsql
│       │       ├── consultar_accesos_estadisticas_listado_centro_adaptacion.zsql
│       │       ├── consultar_accesos_estadisticas_por_afiliacion.zsql
│       │       ├── consultar_accesos_estadisticas_por_afiliacion_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_afiliacion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_categoria_seccion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof_afiliacion.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof_afiliacion_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof_afiliacion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_catprof_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_afiliacion.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_afiliacion_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_afiliacion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_catprof.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_catprof_afiliacion.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_catprof_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_catprof_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_seccion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_centro_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_grupos.zsql
│       │       ├── consultar_accesos_estadisticas_por_meses.zsql
│       │       ├── consultar_accesos_estadisticas_por_meses_TEST.zsql
│       │       ├── consultar_accesos_estadisticas_por_meses_fechas.zsql
│       │       ├── consultar_accesos_estadisticas_por_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_por_seccion_usuario.zsql
│       │       ├── consultar_accesos_estadisticas_por_usuario_afiliacion_seccion
│       │       ├── consultar_accesos_estadisticas_por_usuario_afiliacion_seccion.zsql
│       │       ├── consultar_accesos_estadisticas_usuario_zope.zsql
│       │       ├── consultar_accesos_estadisticas_usuario_zope_SG.zsql
│       │       ├── obtenerCodigosUnicos_afiliados_OBSOLETO.zsql
│       │       ├── v_dafin_pers_centro_relacion_masivo_OBSOLETO.zsql
│       │       └── zsql_mas_visitados.zsql
│       ├── skins.zcml
│       ├── tests
│       │   ├── __init__.py
│       │   ├── base.py
│       │   └── test_doctest.py
│       └── tool.gif
├── docs
│   ├── HISTORY.txt
│   ├── INSTALL.txt
│   ├── LICENSE.GPL
│   └── LICENSE.txt
├── setup.cfg
└── setup.py

``` 
