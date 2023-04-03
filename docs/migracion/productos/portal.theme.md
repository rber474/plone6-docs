---
hide:
  - tags
---

T|Estado|Pendiente|class:pending|
L|:simple-bitbucket:|https://bitbucket.once.es/projects/WEB-PORTAL/repos/portal.theme|
# portal.theme


## Repositorios

| Repositorio                  | URL                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------- |
| :simple-subversion: SVN      | [portal.theme](https://evosvn.evonceib.local/svn/webs/empleado/portal.theme)  |
| :simple-bitbucket: Bitbucket | [portal.theme](ssh://git@bitbucket.once.es:7999/web-portal/portal.theme.git)  |

## Descripción

Paquete de estadísticas para bibliotecas


!!! warning "Atención"

    - Existen múltiples ``ZSQL Methods`` que no están migrados!


## Migración


### Tipos de contenido

Ninguno

### Eventos

Este paquete tiene eventos registrados tales como
Reordenación de objectos en la carpeta tras su creación 

### Adaptadores

EmailVerification -> Preguntar por lógica 

UserValidation -> Preguntar por lógica 

### Indices y metadatos personalizados

Ninguno

### Workflow definitions

No hay workflow específico.

### Profiles

cssregistry.xml -> Conversión a registry resources 

jsregistry.xml -> Conversión a registry resources 

kssregistry.xml -> Eliminación 

metadata.xml -> Revisar dependencia anómala 

portal.theme_various.txt -> Preguntar si realmente se está utilizando 

properties.xml -> Valorar si algunas de esas propiedades pueden estar en un panel de control (validate_email, enable_permalink, left_slots,) 

propertiestool.xml -> Valorar si las propiedades deben estar en el panel de control y modificar las que ya no formen parte de la lógica de Plone6. 

    Ejemplo: 
```
<property name="types_not_searched" type="lines"> 

  <element value="ATBooleanCriterion"/> 

  <element value="ATCurrentAuthorCriterion"/> 

  <element value="ATDateCriteria"/> 
``` 
 

 

rolemap.xml -> Cambiar la asignación de permission para dexterity en el caso de ATContentTypes 

skins.xml -> No procede en Plone 6 


Types - > Eliminar Topic y substituir por Collection


### portal_skins

Contiene múltiples scripts :simple-python: *.py, scritps :simple-jquery: JQuery y múltiples templates.

portal_them_custom_ecmascript -> Trasladar javascripts a THEME/JS 

portal_theme_styles -> Trasladar estilos a THEME/CSS 

portal_theme_fonts -> Trasladar fuentes a THEME/FONTS 

portal_theme_custom_images -> Trasladar imágenes a THEME/IMG 

portal_theme_zsql -> Preguntar por correspondencia en la nueva lógica Plone 6 

portal_theme_custom_scripts -> Migraciones de scripts a views que irán alojadas en BROWSER 

portal_theme_custom_templates -> Revisar una a una las diferentes plantillas  

Distinguir cuales realmente deben ir alojadas en BROWSER/OVERRIDES  

Algunas de ellas se podrían omitir y buscar otras formas de conseguir el mismo efecto sin necesidad de tocar el core de plone. Uso de diazo 


### Árbol de contenidos
```
portal.theme/
├── MANIFEST.in
├── README.txt
├── docs
│   ├── HISTORY.txt
│   ├── INSTALL.txt
│   ├── LICENSE.GPL
│   └── LICENSE.txt
├── portal
│   ├── __init__.py
│   └── theme
│       ├── DetectMobileBrowser.py
│       ├── __init__.py
│       ├── adapter
│       │   ├── __init__.py
│       │   ├── adapter.py
│       │   └── configure.zcml
│       ├── browser
│       │   ├── Anterior
│       │   │   ├── banner_footer.pt
│       │   │   ├── configure.zcml
│       │   │   ├── empleado_author.pt
│       │   │   ├── empleado_cabecera_video.pt
│       │   │   ├── empleado_cabecera_video.pt.metadata
│       │   │   ├── empleado_clearfix.pt
│       │   │   ├── empleado_contentactions.pt
│       │   │   ├── empleado_contentactions_blank.pt
│       │   │   ├── empleado_contentviews.pt
│       │   │   ├── empleado_document_actions.pt
│       │   │   ├── empleado_link_navigation.pt
│       │   │   ├── empleado_review.pt
│       │   │   ├── footer.pt
│       │   │   ├── footer_enlace.pt
│       │   │   ├── gestion_usuario_portlet.pt
│       │   │   ├── global_sections.pt
│       │   │   ├── interfaces.py
│       │   │   ├── kss_sharing.py
│       │   │   ├── localroles.py
│       │   │   ├── logo.pt
│       │   │   ├── logo.pt.metadata
│       │   │   ├── macro_wrapper.pt
│       │   │   ├── my_column.pt
│       │   │   ├── my_error_message.pt
│       │   │   ├── my_foldercontents.pt
│       │   │   ├── my_folderfactories.pt
│       │   │   ├── my_link_search.pt
│       │   │   ├── my_navigation_recurse.pt
│       │   │   ├── my_review_history.pt
│       │   │   ├── navigation.pt
│       │   │   ├── navigation.py
│       │   │   ├── path_bar.pt
│       │   │   ├── personal_bar.pt
│       │   │   ├── portal_topbar.pt
│       │   │   ├── portlet.py
│       │   │   ├── review.py
│       │   │   ├── search.py
│       │   │   ├── searchbox.pt
│       │   │   ├── sharing.pt
│       │   │   ├── sharing.py
│       │   │   ├── site_actions.pt
│       │   │   ├── skip_links.pt
│       │   │   ├── viewlet.pt
│       │   │   ├── viewlets.py
│       │   │   └── viewlets.zcml
│       │   ├── SinAccesoGestiona.py
│       │   ├── WS
│       │   │   ├── SOAPCampusView.py
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   └── pruebaWS.py
│       │   ├── __init__.py
│       │   ├── account.py
│       │   ├── buscadorView.py
│       │   ├── configure.zcml
│       │   ├── controlpanels
│       │   │   ├── __init__.py
│       │   │   ├── configure.zcml
│       │   │   ├── interfaces.py
│       │   │   ├── portletscontrolpanel.py
│       │   │   ├── templates
│       │   │   │   └── controlpanel_layout.pt
│       │   │   └── vocabularies.py
│       │   ├── datecomponents.py
│       │   ├── extractUserInfoFromCodUnicoCsvForm.py
│       │   ├── fixWrongDataInPortEstadisticaAcceso.py
│       │   ├── interfaces.py
│       │   ├── mobile_detector.py
│       │   ├── overrides
│       │   │   ├── __init__.py
│       │   │   ├── account-panel-bare.pt
│       │   │   ├── cancel.pt
│       │   │   ├── checkin.pt
│       │   │   ├── checkout.pt
│       │   │   ├── collective.z3cform.datetimewidget.templates.datetime_input.pt
│       │   │   ├── constraintypes.pt
│       │   │   ├── constraintypes.py
│       │   │   ├── contentmenu.pt
│       │   │   ├── control.py
│       │   │   ├── dexterityfixes.py
│       │   │   ├── diff.pt
│       │   │   ├── discussion.css
│       │   │   ├── edit.pt
│       │   │   ├── folderfactories.pt
│       │   │   ├── folderfactories.py
│       │   │   ├── newuser_form.pt
│       │   │   ├── pageform.pt
│       │   │   ├── plone.app.z3cform.templates.object_input.pt
│       │   │   ├── plone.app.z3cform.templates.widget.pt
│       │   │   ├── register.py
│       │   │   ├── register_form.pt
│       │   │   ├── search.js
│       │   │   ├── search.pt
│       │   │   ├── search.py
│       │   │   ├── sitemap-item.pt
│       │   │   ├── sitemap.pt
│       │   │   ├── sitemap.pt.metadata
│       │   │   ├── sitemap.py
│       │   │   ├── subpageform.pt
│       │   │   └── updated_search.pt
│       │   ├── portalthemeview.py
│       │   ├── portlets
│       │   │   ├── FavoritosPortlet.py
│       │   │   ├── GestionaPortlet.py
│       │   │   ├── KssPortlets.py
│       │   │   ├── PortletRevision.py
│       │   │   ├── SuscripcionesPortlet.py
│       │   │   ├── ValidarPersonasPortlet.py
│       │   │   ├── __init__.py
│       │   │   ├── avisosGestionaPortlet.py
│       │   │   ├── configure.zcml
│       │   │   ├── dashboard.py
│       │   │   ├── dashboard_vistaPortlet.py
│       │   │   ├── datosPersonales.py
│       │   │   ├── deferredRevisionPortlet.py
│       │   │   ├── manager.py
│       │   │   ├── navigation.py
│       │   │   ├── navtree.py
│       │   │   ├── suscripciones_summary_view.py
│       │   │   ├── templates
│       │   │   │   ├── CargaPortletRevision.pt
│       │   │   │   ├── DashboardVistaPortlet.pt
│       │   │   │   ├── DatosPersonalesPortlet.pt
│       │   │   │   ├── FavoritosPortlet.pt
│       │   │   │   ├── PortletRevision.pt
│       │   │   │   ├── PortletRevisionPrivadoDashboard.pt
│       │   │   │   ├── SuscripcionesPortlet.pt
│       │   │   │   ├── ValidarPersonasPortlet.pt
│       │   │   │   ├── avisosGestionaPortlet.pt
│       │   │   │   ├── dashboard-column.pt
│       │   │   │   ├── dashboard.pt
│       │   │   │   ├── dashboard_vista.pt
│       │   │   │   ├── error_message.pt
│       │   │   │   ├── gestionaPortlet.pt
│       │   │   │   ├── gestor1.pt
│       │   │   │   ├── gestordeportlets1.pt
│       │   │   │   ├── manage-dashboard.pt
│       │   │   │   ├── my_navigation_recurse.pt
│       │   │   │   ├── my_navigation_recurse_mobile.pt
│       │   │   │   ├── navigation.pt
│       │   │   │   ├── suscripciones_summary_view.pt
│       │   │   │   ├── toggleViewPortlet.pt
│       │   │   │   ├── zona_personal.pt
│       │   │   │   └── zona_personal.pt.metadata
│       │   │   ├── toggleViewPortlet.py
│       │   │   └── zonaPersonal.py
│       │   ├── resources
│       │   │   ├── android-chrome-192x192.png
│       │   │   ├── android-chrome-512x512.png
│       │   │   ├── fa-brands-400.eot
│       │   │   ├── fa-brands-400.svg
│       │   │   ├── fa-brands-400.ttf
│       │   │   ├── fa-brands-400.woff
│       │   │   ├── fa-brands-400.woff2
│       │   │   ├── fa-regular-400.eot
│       │   │   ├── fa-regular-400.svg
│       │   │   ├── fa-regular-400.ttf
│       │   │   ├── fa-regular-400.woff
│       │   │   ├── fa-regular-400.woff2
│       │   │   ├── fa-solid-900.eot
│       │   │   ├── fa-solid-900.svg
│       │   │   ├── fa-solid-900.ttf
│       │   │   ├── fa-solid-900.woff
│       │   │   ├── fa-solid-900.woff2
│       │   │   ├── favicon-16x16.png
│       │   │   ├── favicon-32x32.png
│       │   │   ├── favicon.ico
│       │   │   ├── manifest.json
│       │   │   ├── overlayhelpers.js
│       │   │   ├── portaltheme.kss
│       │   │   └── touch_icon.png
│       │   ├── rotarImagenView.py
│       │   ├── templates
│       │   │   ├── SinAccesoGestiona.pt
│       │   │   ├── __init__.py
│       │   │   ├── buscador_contenido.pt
│       │   │   ├── extract_user_info_form.pt
│       │   │   ├── fix_wrong_afilia_data_in_port_estadistica.pt
│       │   │   └── validate_user.pt
│       │   ├── validate_user.py
│       │   ├── validator.py
│       │   └── viewlets
│       │       ├── __init__.py
│       │       ├── browser.py
│       │       ├── configure.zcml
│       │       ├── localroles.py
│       │       ├── templates
│       │       │   ├── batch_macros.pt
│       │       │   ├── batchnavigation.pt
│       │       │   ├── bottom_site_actions.pt
│       │       │   ├── contentmenu.pt
│       │       │   ├── default_error_message.pt
│       │       │   ├── empleado_author.pt
│       │       │   ├── empleado_clearfix.pt
│       │       │   ├── empleado_contentactions.pt
│       │       │   ├── empleado_contentactions_blank.pt
│       │       │   ├── empleado_contentviews.pt
│       │       │   ├── empleado_document_actions.pt
│       │       │   ├── empleado_foldercontents.pt
│       │       │   ├── favicon.pt
│       │       │   ├── folder_contents.js
│       │       │   ├── footer.pt
│       │       │   ├── footer_site_actions.pt
│       │       │   ├── general_logo_mobile.pt
│       │       │   ├── general_mobile_bar.pt
│       │       │   ├── global_sections.pt
│       │       │   ├── header_block_right.pt
│       │       │   ├── keywords.pt
│       │       │   ├── leytransparencia.pt
│       │       │   ├── logo.pt
│       │       │   ├── mail_password_form.pt
│       │       │   ├── my_link_search.pt
│       │       │   ├── my_review_history.pt
│       │       │   ├── path_bar.pt
│       │       │   ├── path_bar_para_biblioteca.pt
│       │       │   ├── personal_bar.pt
│       │       │   ├── portal_h1oficina.pt
│       │       │   ├── portal_middleheader.pt
│       │       │   ├── portal_searchbox.pt
│       │       │   ├── portal_topbar.pt
│       │       │   ├── portal_topheader.pt
│       │       │   ├── site_actions.pt
│       │       │   ├── skip_links.pt
│       │       │   ├── skip_links_mobile.pt
│       │       │   ├── table.pt
│       │       │   └── title.pt
│       │       ├── viewlets.py
│       │       └── viewlets_pendientes_revision.py
│       ├── clamAVOverrides
│       │   ├── __init__.py
│       │   ├── configure.zcml
│       │   └── validators.py
│       ├── config.py
│       ├── configure.zcml
│       ├── events
│       │   ├── __init__.py
│       │   ├── configure.zcml
│       │   └── handlers.py
│       ├── extender.py
│       ├── locales
│       │   ├── README.txt
│       │   └── es
│       │       └── LC_MESSAGES
│       │           ├── atcontenttypes.mo
│       │           ├── atcontenttypes.po
│       │           ├── atreferencebrowserwidget.mo
│       │           ├── atreferencebrowserwidget.po
│       │           ├── cmfeditions.mo
│       │           ├── cmfeditions.po
│       │           ├── cmfnotification.po
│       │           ├── cmfplacefulworkflow.mo
│       │           ├── cmfplacefulworkflow.po
│       │           ├── contentratings.mo
│       │           ├── contentratings.po
│       │           ├── linguaplone.mo
│       │           ├── linguaplone.po
│       │           ├── oficina.mo
│       │           ├── oficina.po
│       │           ├── passwordresettool.mo
│       │           ├── passwordresettool.po
│       │           ├── plone.contentratings.mo
│       │           ├── plone.contentratings.po
│       │           ├── plone.po
│       │           ├── plonefrontpage.mo
│       │           ├── plonefrontpage.po
│       │           ├── plonelocales.mo
│       │           ├── plonelocales.po
│       │           ├── portal.theme.po
│       │           ├── simpleattachment.mo
│       │           ├── simpleattachment.po
│       │           ├── z3c.form.mo
│       │           ├── z3c.form.po
│       │           ├── zope.mo
│       │           └── zope.po
│       ├── overrides.zcml
│       ├── patches.py
│       ├── permissions.py
│       ├── portalTool.py
│       ├── profiles
│       │   ├── default
│       │   │   ├── actions.xml
│       │   │   ├── browserlayer.xml
│       │   │   ├── controlpanel.xml
│       │   │   ├── cssregistry.xml
│       │   │   ├── import_steps.xml
│       │   │   ├── jsregistry.xml
│       │   │   ├── kssregistry.xml
│       │   │   ├── metadata.xml
│       │   │   ├── mimetypes.xml
│       │   │   ├── portal.theme_various.txt
│       │   │   ├── portlets.xml
│       │   │   ├── properties.xml
│       │   │   ├── propertiestool.xml
│       │   │   ├── registry.xml
│       │   │   ├── rolemap.xml
│       │   │   ├── skins.xml
│       │   │   ├── types
│       │   │   │   ├── Collection.xml
│       │   │   │   ├── Document.xml
│       │   │   │   ├── Event.xml
│       │   │   │   ├── File.xml
│       │   │   │   ├── Folder.xml
│       │   │   │   ├── Image.xml
│       │   │   │   ├── Link.xml
│       │   │   │   ├── News_Item.xml
│       │   │   │   └── Topic.xml
│       │   │   └── viewlets.xml
│       │   └── uninstall
│       │       └── viewlets.xml
│       ├── profiles.zcml
│       ├── setuphandlers.py
│       ├── skins
│       │   ├── portal_them_custom_ecmascript
│       │   │   ├── calendar.js
│       │   │   ├── calendar_formfield.js
│       │   │   ├── calendar_stripped.js
│       │   │   ├── carousel-portalonce.js
│       │   │   ├── datagridwidget.js
│       │   │   ├── dragdropreorder.js
│       │   │   ├── dropdown.js
│       │   │   ├── focosie7.js
│       │   │   ├── form_tabbing.js
│       │   │   ├── get-sdk-script.js
│       │   │   ├── googleAnalytics-min.js
│       │   │   ├── html5shiv-printshiv.js
│       │   │   ├── html5shiv.js
│       │   │   ├── inbenta-conf.min.js
│       │   │   ├── jquery.highlightsearchterms.js
│       │   │   ├── kss-bbb.js
│       │   │   ├── login.js
│       │   │   ├── pseudofocus.js
│       │   │   └── referencebrowser.js
│       │   ├── portal_theme_custom_images
│       │   │   ├── 400p.gif
│       │   │   ├── 480p.gif
│       │   │   ├── 500p.gif
│       │   │   ├── 600p.gif
│       │   │   ├── 700p.gif
│       │   │   ├── 768p.gif
│       │   │   ├── 800p.gif
│       │   │   ├── BC_flecha.gif
│       │   │   ├── BC_flecha2.gif
│       │   │   ├── BC_flecha3.gif
│       │   │   ├── aceptada-icon.png
│       │   │   ├── alert_home.png
│       │   │   ├── alternativa1
│       │   │   │   ├── ayuda.gif
│       │   │   │   ├── button.png
│       │   │   │   ├── ceo.png
│       │   │   │   ├── contraste.gif
│       │   │   │   ├── flecha-abajo.jpg
│       │   │   │   ├── flecha-arriba.jpg
│       │   │   │   ├── flecha-derecha.jpg
│       │   │   │   ├── fnd_gestiona.jpg
│       │   │   │   ├── fnd_micentro.jpg
│       │   │   │   ├── gestiona.png
│       │   │   │   ├── herramientas.gif
│       │   │   │   ├── herramientas.png
│       │   │   │   ├── ico_dicc.png
│       │   │   │   ├── ico_direc.png
│       │   │   │   ├── ico_fav.png
│       │   │   │   ├── ico_links.png
│       │   │   │   ├── ico_map.png
│       │   │   │   ├── ico_tiempo.png
│       │   │   │   ├── ico_zonapers.png
│       │   │   │   ├── mail.gif
│       │   │   │   ├── mail.png
│       │   │   │   ├── mapaweb.gif
│       │   │   │   ├── menu1.jpg
│       │   │   │   ├── nav_fnd.jpg
│       │   │   │   ├── order_down.png
│       │   │   │   ├── order_down_hover.png
│       │   │   │   ├── order_up.png
│       │   │   │   ├── order_up_hover.png
│       │   │   │   ├── pictograma-ONCE-asisomos.png
│       │   │   │   ├── portlet-imagen-gestiona.jpg
│       │   │   │   ├── search_down.png
│       │   │   │   ├── search_down_hover.png
│       │   │   │   ├── search_up.png
│       │   │   │   ├── search_up_hover.png
│       │   │   │   ├── sede.png
│       │   │   │   └── tablon.png
│       │   │   ├── altocontraste
│       │   │   │   ├── BC_flecha.gif
│       │   │   │   ├── aceptada-icon.png
│       │   │   │   ├── alert_home.png
│       │   │   │   ├── ayuda.gif
│       │   │   │   ├── back.png
│       │   │   │   ├── blank.gif
│       │   │   │   ├── button.png
│       │   │   │   ├── ceo.png
│       │   │   │   ├── contraste.gif
│       │   │   │   ├── denegada-icon.png
│       │   │   │   ├── download_icon.png
│       │   │   │   ├── download_icon_focus.png
│       │   │   │   ├── en-tramite-icon.png
│       │   │   │   ├── fav.png
│       │   │   │   ├── flecha-abajo.jpg
│       │   │   │   ├── flecha-arriba.jpg
│       │   │   │   ├── flecha-derecha.jpg
│       │   │   │   ├── flecha.gif
│       │   │   │   ├── fnd.jpg
│       │   │   │   ├── fnd_form.gif
│       │   │   │   ├── fnd_gestiona.jpg
│       │   │   │   ├── fnd_micentro.jpg
│       │   │   │   ├── fnd_noticias.jpg
│       │   │   │   ├── fnd_tablon.jpg
│       │   │   │   ├── fnd_zonapersonal.jpg
│       │   │   │   ├── fnd_zonapersonal_OLD2.jpg
│       │   │   │   ├── foto.jpg
│       │   │   │   ├── gestiona.png
│       │   │   │   ├── herramientas.gif
│       │   │   │   ├── herramientas.png
│       │   │   │   ├── ico_alert.png
│       │   │   │   ├── ico_dicc.png
│       │   │   │   ├── ico_direc.png
│       │   │   │   ├── ico_doc.gif
│       │   │   │   ├── ico_fav.png
│       │   │   │   ├── ico_int_bombilla.png
│       │   │   │   ├── ico_int_editar.png
│       │   │   │   ├── ico_int_encuesta.png
│       │   │   │   ├── ico_int_lupa.png
│       │   │   │   ├── ico_links.png
│       │   │   │   ├── ico_map.png
│       │   │   │   ├── ico_mi-carrito.png
│       │   │   │   ├── ico_mw1.png
│       │   │   │   ├── ico_mw2.gif
│       │   │   │   ├── ico_tiempo.png
│       │   │   │   ├── ico_zonapers.png
│       │   │   │   ├── icon_ext_excel.png
│       │   │   │   ├── icon_ext_libreta.png
│       │   │   │   ├── icon_ext_mensajero.png
│       │   │   │   ├── icon_ext_periodico.png
│       │   │   │   ├── icon_ext_semaforo.png
│       │   │   │   ├── icon_ext_sobre.png
│       │   │   │   ├── icon_extra_imagencabecera.png
│       │   │   │   ├── logo.png
│       │   │   │   ├── mail.gif
│       │   │   │   ├── mail.png
│       │   │   │   ├── mapaweb.gif
│       │   │   │   ├── mas.png
│       │   │   │   ├── menos.png
│       │   │   │   ├── menu.png
│       │   │   │   ├── menu1.jpg
│       │   │   │   ├── menu2.jpg
│       │   │   │   ├── menu3.jpg
│       │   │   │   ├── nav_fnd.jpg
│       │   │   │   ├── order_down.png
│       │   │   │   ├── order_down_hover.png
│       │   │   │   ├── order_up.png
│       │   │   │   ├── order_up_hover.png
│       │   │   │   ├── paleta.gif
│       │   │   │   ├── pictograma-ONCE-asisomos.png
│       │   │   │   ├── politica-de-privacidad.png
│       │   │   │   ├── popup_calendar.png
│       │   │   │   ├── portlet-imagen-gestiona.jpg
│       │   │   │   ├── portlet-imagen-gestiona_OLD.jpg
│       │   │   │   ├── portlet-imagen-zpersonal.jpg
│       │   │   │   ├── portlet-imagen-zpersonal_OLD.jpg
│       │   │   │   ├── print.png
│       │   │   │   ├── search.gif
│       │   │   │   ├── search_down.png
│       │   │   │   ├── search_down_hover.png
│       │   │   │   ├── search_up.png
│       │   │   │   ├── search_up_hover.png
│       │   │   │   ├── sede.png
│       │   │   │   ├── stars.gif
│       │   │   │   ├── stars.png
│       │   │   │   ├── sug_respuesta.png
│       │   │   │   ├── sugerencias_int.jpg
│       │   │   │   ├── tablon.png
│       │   │   │   ├── up.png
│       │   │   │   ├── user.png
│       │   │   │   ├── vineta.gif
│       │   │   │   ├── vote_maybe.png
│       │   │   │   ├── vote_no.png
│       │   │   │   ├── vote_yes.png
│       │   │   │   ├── zp-edit-icon.png
│       │   │   │   ├── zp-mas-icon.png
│       │   │   │   └── zp-menos-icon.png
│       │   │   ├── aniversario.png
│       │   │   ├── application.png
│       │   │   ├── audio.png
│       │   │   ├── ayuda.gif
│       │   │   ├── back.png
│       │   │   ├── banner.jpg
│       │   │   ├── blank.gif
│       │   │   ├── blue_line.png
│       │   │   ├── boletin
│       │   │   │   ├── flecha.png
│       │   │   │   ├── flecha1.png
│       │   │   │   ├── flecha2.png
│       │   │   │   ├── flecha3.png
│       │   │   │   ├── icon_extra_imagencabecera.png
│       │   │   │   └── lineaCabecera.png
│       │   │   ├── bra.png
│       │   │   ├── button.png
│       │   │   ├── comment_edit.png
│       │   │   ├── contraste.gif
│       │   │   ├── corner_post.png
│       │   │   ├── denegada-icon.png
│       │   │   ├── desplegar
│       │   │   │   ├── order_down.png
│       │   │   │   ├── order_down_hover.png
│       │   │   │   ├── order_up.png
│       │   │   │   ├── order_up_hover.png
│       │   │   │   ├── search_down.png
│       │   │   │   ├── search_down_hover.png
│       │   │   │   ├── search_up.png
│       │   │   │   └── search_up_hover.png
│       │   │   ├── destacados.gif
│       │   │   ├── doc.gif
│       │   │   ├── doc.png
│       │   │   ├── document-actions
│       │   │   │   ├── des.png
│       │   │   │   ├── fav.png
│       │   │   │   ├── flecha.gif
│       │   │   │   ├── mail.png
│       │   │   │   ├── menu.png
│       │   │   │   ├── nofav.gif
│       │   │   │   ├── print.png
│       │   │   │   ├── question.png
│       │   │   │   └── rss.png
│       │   │   ├── doublearrow.png
│       │   │   ├── download_icon.png
│       │   │   ├── download_icon_focus.png
│       │   │   ├── en-tramite-icon.png
│       │   │   ├── encabezados
│       │   │   │   ├── fnd_gestiona.jpg
│       │   │   │   ├── fnd_micentro.jpg
│       │   │   │   ├── fnd_noticias.jpg
│       │   │   │   ├── fnd_zonapersonal.jpg
│       │   │   │   ├── portlet-imagen-gestiona.jpg
│       │   │   │   └── portlet-imagen-zpersonal.jpg
│       │   │   ├── external_link_blank.png
│       │   │   ├── external_link_blank_old.png
│       │   │   ├── flecha.gif
│       │   │   ├── fnd.jpg
│       │   │   ├── fnd_gestiona.jpg
│       │   │   ├── fnd_micentro.jpg
│       │   │   ├── fnd_noticias.jpg
│       │   │   ├── fnd_tablon.jpg
│       │   │   ├── fnd_zonapersonal.jpg
│       │   │   ├── foto.jpg
│       │   │   ├── gs-once.png
│       │   │   ├── herramientas.gif
│       │   │   ├── ico-blo-color.png
│       │   │   ├── ico-blo-gray.png
│       │   │   ├── ico-fb-color.png
│       │   │   ├── ico-fb-gray.png
│       │   │   ├── ico-in-color.png
│       │   │   ├── ico-in-gray.png
│       │   │   ├── ico-lin-color.png
│       │   │   ├── ico-lin-gray.png
│       │   │   ├── ico-tw-color.png
│       │   │   ├── ico-tw-gray.png
│       │   │   ├── ico-yb-color.png
│       │   │   ├── ico-yb-gray.png
│       │   │   ├── ico_alert.png
│       │   │   ├── ico_calendar.gif
│       │   │   ├── ico_doc.gif
│       │   │   ├── ico_int_bombilla.png
│       │   │   ├── ico_int_editar.png
│       │   │   ├── ico_int_encuesta.png
│       │   │   ├── ico_int_lupa.png
│       │   │   ├── ico_mi-carrito.png
│       │   │   ├── ico_mw1.png
│       │   │   ├── ico_mw2.png
│       │   │   ├── ico_unread.png
│       │   │   ├── ico_xls.png
│       │   │   ├── icon_ext_excel.png
│       │   │   ├── icon_ext_libreta.png
│       │   │   ├── icon_ext_mensajero.png
│       │   │   ├── icon_ext_periodico.png
│       │   │   ├── icon_ext_semaforo.png
│       │   │   ├── icon_ext_sobre.png
│       │   │   ├── icons.gif
│       │   │   ├── image.png
│       │   │   ├── image_icon.gif
│       │   │   ├── imagen-transparente.gif
│       │   │   ├── internal_link_blank.png
│       │   │   ├── linea-deco-banner-home.png
│       │   │   ├── linea-footer.png
│       │   │   ├── listacorreo.gif
│       │   │   ├── listadestacados.gif
│       │   │   ├── listaemail.gif
│       │   │   ├── listaenlaces.gif
│       │   │   ├── listaenlaceslogo.gif
│       │   │   ├── listaenviar.gif
│       │   │   ├── listaperfiles.gif
│       │   │   ├── listatelefono.gif
│       │   │   ├── logo.png
│       │   │   ├── mail.gif
│       │   │   ├── mapaweb.gif
│       │   │   ├── mas.png
│       │   │   ├── menos.png
│       │   │   ├── menu.png
│       │   │   ├── mob
│       │   │   │   ├── bullets.png
│       │   │   │   ├── flecha-abajo.jpg
│       │   │   │   ├── flecha-arriba.jpg
│       │   │   │   ├── flecha-derecha.jpg
│       │   │   │   ├── menu1.jpg
│       │   │   │   ├── menu2.jpg
│       │   │   │   ├── menu3.jpg
│       │   │   │   ├── mob_buscar.png
│       │   │   │   ├── mob_buscar_hover.png
│       │   │   │   ├── mob_gestiona.png
│       │   │   │   ├── mob_gestiona_hover.png
│       │   │   │   ├── mob_mas.png
│       │   │   │   ├── mob_mas_hover.png
│       │   │   │   ├── mob_menu.png
│       │   │   │   ├── mob_menu_hover.png
│       │   │   │   ├── mob_zonapersonal.png
│       │   │   │   └── mob_zonapersonal_hover.png
│       │   │   ├── next.png
│       │   │   ├── paleta.gif
│       │   │   ├── pause-slides-blue.png
│       │   │   ├── pdf.gif
│       │   │   ├── pdf.png
│       │   │   ├── play-button-blue.png
│       │   │   ├── play-slides-blue.png
│       │   │   ├── png.png
│       │   │   ├── popup_calendar.png
│       │   │   ├── prev.png
│       │   │   ├── rar.png
│       │   │   ├── rtf.gif
│       │   │   ├── search.gif
│       │   │   ├── semaforo.jpg
│       │   │   ├── site-actions
│       │   │   │   ├── ayuda.gif
│       │   │   │   ├── contraste.gif
│       │   │   │   ├── gestiona.png
│       │   │   │   ├── herramientas.gif
│       │   │   │   ├── herramientas.png
│       │   │   │   ├── ico_mi-carrito.png
│       │   │   │   ├── mail.gif
│       │   │   │   ├── mail.png
│       │   │   │   ├── mapaweb.gif
│       │   │   │   ├── politica-de-privacidad.png
│       │   │   │   └── tablon.png
│       │   │   ├── site-actions2
│       │   │   │   ├── ceo.png
│       │   │   │   ├── fnd_form.gif
│       │   │   │   ├── ico_alert.png
│       │   │   │   ├── ico_dicc.png
│       │   │   │   ├── ico_direc.png
│       │   │   │   ├── ico_fav.png
│       │   │   │   ├── ico_links.png
│       │   │   │   ├── ico_map.png
│       │   │   │   ├── ico_tiempo.png
│       │   │   │   ├── ico_zonapers.png
│       │   │   │   ├── search.gif
│       │   │   │   └── sede.png
│       │   │   ├── stars.gif
│       │   │   ├── sug_respuesta.png
│       │   │   ├── sugerencias_int.jpg
│       │   │   ├── triangle-white-up.png
│       │   │   ├── up.png
│       │   │   ├── user.png
│       │   │   ├── video.png
│       │   │   ├── vineta.gif
│       │   │   ├── visorampliar.gif
│       │   │   ├── visorampliarblanco.gif
│       │   │   ├── vocacion-social.png
│       │   │   ├── vote_maybe.png
│       │   │   ├── vote_no.png
│       │   │   ├── vote_yes.png
│       │   │   ├── xls.gif
│       │   │   ├── xls.png
│       │   │   ├── zip.png
│       │   │   ├── zp-edit-icon.png
│       │   │   ├── zp-mas-icon.png
│       │   │   └── zp-menos-icon.png
│       │   ├── portal_theme_custom_scripts
│       │   │   ├── actualizar_extensiones_portal.py
│       │   │   ├── actualizar_seguridad_ficheros.py
│       │   │   ├── at_download.py
│       │   │   ├── borrar_grupos_TPV.py
│       │   │   ├── campus_definir_email_script.cpy
│       │   │   ├── campus_definir_email_script.cpy.metadata
│       │   │   ├── compararRangosIPs.py
│       │   │   ├── consultaSesion.py
│       │   │   ├── content_status_modify.cpy
│       │   │   ├── content_status_modify.cpy.metadata
│       │   │   ├── crearCookieVersionEscritorio.py
│       │   │   ├── date_components_support.py
│       │   │   ├── delete_confirmation.cpy
│       │   │   ├── delete_confirmation.cpy.metadata
│       │   │   ├── eliminar_emails_inexistentes.py
│       │   │   ├── exportarUsuariosSuscritos.py
│       │   │   ├── folder_cut.cpy
│       │   │   ├── folder_cut.cpy.metadata
│       │   │   ├── folder_delete.cpy
│       │   │   ├── folder_delete.cpy.metadata
│       │   │   ├── folder_delete_post_confirmation.cpy
│       │   │   ├── folder_delete_post_confirmation.cpy.metadata
│       │   │   ├── folder_paste.cpy
│       │   │   ├── folder_paste.cpy.metadata
│       │   │   ├── getBestIcon.py
│       │   │   ├── getPathbar.py
│       │   │   ├── get_ip.py
│       │   │   ├── go_back.cpy
│       │   │   ├── insertarAccesoSeguridadArchivo.py
│       │   │   ├── insertarAccesoSeguridadImagen.py
│       │   │   ├── link_redirect_view.py
│       │   │   ├── listar_ficheros_alta_seguridad.py
│       │   │   ├── login.py
│       │   │   ├── login_next.cpy
│       │   │   ├── login_next.cpy.metadata
│       │   │   ├── mail_password.py.metadata_OLD
│       │   │   ├── mail_password.py_OLD
│       │   │   ├── object_cut.cpy
│       │   │   ├── object_cut.cpy.metadata
│       │   │   ├── object_paste.cpy
│       │   │   ├── object_paste.cpy.metadata
│       │   │   ├── obtenerGrupoUsuario.py
│       │   │   ├── obtenerIdSesion.py
│       │   │   ├── obtenerPermisoAnuncio.py
│       │   │   ├── obtenerSiAutenticado.py
│       │   │   ├── permisos_insuficientes.py
│       │   │   ├── registro_acceso.py
│       │   │   ├── removeInvalidUsersSubscriptions.py
│       │   │   ├── require_login.py
│       │   │   ├── scriptCentros.py
│       │   │   ├── send_feedback_site.cpy
│       │   │   ├── send_feedback_site.cpy.metadata
│       │   │   ├── sitemap1.py
│       │   │   └── validate_pwreset_password.vpy
│       │   ├── portal_theme_custom_templates
│       │   │   ├── accessibility-info.pt
│       │   │   ├── attachment_widgets
│       │   │   │   ├── widget_attachmentsmanager.pt
│       │   │   │   └── widget_imagesmanager.pt
│       │   │   ├── ayuda.pt
│       │   │   ├── base_edit.cpt
│       │   │   ├── base_edit.cpt.metadata
│       │   │   ├── base_view.pt
│       │   │   ├── base_view.pt.metadata
│       │   │   ├── batch_macros.pt
│       │   │   ├── batchnavigation.pt
│       │   │   ├── calendar_macros.pt
│       │   │   ├── campus_definir_email.cpt
│       │   │   ├── campus_definir_email.cpt.metadata
│       │   │   ├── contact-info.cpt
│       │   │   ├── contact-info.cpt.metadata
│       │   │   ├── content_status_history.cpt
│       │   │   ├── content_status_history.cpt.metadata
│       │   │   ├── datagrid_text_cell.pt
│       │   │   ├── datagridwidget.pt
│       │   │   ├── datagridwidget_edit_row.pt
│       │   │   ├── datagridwidget_manipulators.pt
│       │   │   ├── default_error_message.pt_OLD
│       │   │   ├── delete_confirmation_content.cpt
│       │   │   ├── delete_confirmation_content.cpt.metadata
│       │   │   ├── delete_confirmation_page.cpt
│       │   │   ├── delete_confirmation_page.cpt.metadata
│       │   │   ├── edit_macros.pt
│       │   │   ├── field.pt__COMPROBAR
│       │   │   ├── folder_full_view.pt
│       │   │   ├── folder_full_view.pt.metadata
│       │   │   ├── folder_full_view_item.pt
│       │   │   ├── folder_listing.pt
│       │   │   ├── folder_listing.pt.metadata
│       │   │   ├── folder_rename_form.cpt
│       │   │   ├── folder_rename_form.cpt.metadata
│       │   │   ├── folder_summary_view.pt
│       │   │   ├── folder_summary_view.pt.metadata
│       │   │   ├── folder_tabular_view.pt
│       │   │   ├── folder_tabular_view.pt.metadata
│       │   │   ├── global_statusmessage.pt
│       │   │   ├── image_view.pt
│       │   │   ├── image_view_fullscreen.pt
│       │   │   ├── insufficient_privileges.pt
│       │   │   ├── kss_generic_macros.pt
│       │   │   ├── link_view.pt
│       │   │   ├── login_success.pt
│       │   │   ├── mail_password_form.pt
│       │   │   ├── mail_password_form.pt.metadata
│       │   │   ├── mail_password_response.pt
│       │   │   ├── mail_password_response.pt.metadata
│       │   │   ├── main_template.pt
│       │   │   ├── main_template_login - backup.pt
│       │   │   ├── main_template_login.pt
│       │   │   ├── my_errors.pt
│       │   │   ├── pwreset_expired.pt
│       │   │   ├── pwreset_expired.pt.metadata
│       │   │   ├── pwreset_finish.pt
│       │   │   ├── pwreset_finish.pt.metadata
│       │   │   ├── pwreset_form.cpt
│       │   │   ├── pwreset_form.cpt.metadata
│       │   │   ├── pwreset_invalid.pt
│       │   │   ├── pwreset_invalid.pt.metadata
│       │   │   ├── referencebrowser.pt
│       │   │   ├── reject_comment.cpt
│       │   │   ├── reject_comment.cpt.metadata
│       │   │   ├── simplevocabulary_view_bk.pt
│       │   │   ├── site_feedback_template.pt
│       │   │   ├── standard_error_message.py
│       │   │   ├── version_diff.pt
│       │   │   ├── widget_attachmentsmanager.pt
│       │   │   ├── widget_imagesmanager.pt
│       │   │   ├── widgets
│       │   │   │   ├── addable_support.pt
│       │   │   │   ├── boolean.pt
│       │   │   │   ├── calendar.pt
│       │   │   │   ├── computed.pt
│       │   │   │   ├── decimal.pt
│       │   │   │   ├── epoz.pt
│       │   │   │   ├── field.pt
│       │   │   │   ├── field_table.pt
│       │   │   │   ├── file.pt
│       │   │   │   ├── image.pt
│       │   │   │   ├── inandout.pt
│       │   │   │   ├── integer.pt
│       │   │   │   ├── js
│       │   │   │   │   ├── inandout.js
│       │   │   │   │   ├── inandout.js.metadata
│       │   │   │   │   ├── keywordmultiselect.js
│       │   │   │   │   ├── picklist.js
│       │   │   │   │   ├── picklist.js.metadata
│       │   │   │   │   ├── textcount.js
│       │   │   │   │   └── textcount.js.metadata
│       │   │   │   ├── keyword.pt
│       │   │   │   ├── label.pt
│       │   │   │   ├── languagewidget.pt
│       │   │   │   ├── lines.pt
│       │   │   │   ├── multiselection.pt
│       │   │   │   ├── password.pt
│       │   │   │   ├── picklist.pt
│       │   │   │   ├── reference.pt
│       │   │   │   ├── rich.pt
│       │   │   │   ├── selection.pt
│       │   │   │   ├── string.pt
│       │   │   │   ├── tests
│       │   │   │   │   ├── keywordmultiselecttests.js
│       │   │   │   │   └── keywordtests.html
│       │   │   │   ├── textarea.pt
│       │   │   │   ├── visual.pt
│       │   │   │   └── zid.pt
│       │   │   └── zona_personal_portlets.pt
│       │   ├── portal_theme_fonts
│       │   │   ├── UsuariosIntranet.txt
│       │   │   ├── texgyreadventor-bold-webfont.eot
│       │   │   ├── texgyreadventor-bold-webfont.svg
│       │   │   ├── texgyreadventor-bold-webfont.ttf
│       │   │   ├── texgyreadventor-bold-webfont.woff
│       │   │   ├── texgyreadventor-bolditalic-webfont.svg
│       │   │   ├── texgyreadventor-bolditalic-webfont.ttf
│       │   │   ├── texgyreadventor-bolditalic-webfont.woff
│       │   │   ├── texgyreadventor-italic-webfont.svg
│       │   │   ├── texgyreadventor-italic-webfont.ttf
│       │   │   ├── texgyreadventor-italic-webfont.woff
│       │   │   ├── texgyreadventor-regular-webfont.eot
│       │   │   ├── texgyreadventor-regular-webfont.svg
│       │   │   ├── texgyreadventor-regular-webfont.ttf
│       │   │   └── texgyreadventor-regular-webfont.woff
│       │   ├── portal_theme_styles
│       │   │   ├── IE9.js
│       │   │   ├── IEFixes.css
│       │   │   ├── RTL.css
│       │   │   ├── _variables.scss
│       │   │   ├── alternativas.css
│       │   │   ├── altocontraste.css
│       │   │   ├── animations.css
│       │   │   ├── asisomos.css
│       │   │   ├── asisomos.scss
│       │   │   ├── authoring.css
│       │   │   ├── base.css
│       │   │   ├── base_properties.props
│       │   │   ├── calendar-system.css
│       │   │   ├── calendario.js
│       │   │   ├── change.js
│       │   │   ├── columns.css
│       │   │   ├── content.css.dtml
│       │   │   ├── contenttree.css
│       │   │   ├── controlpanel.css
│       │   │   ├── deprecated.css
│       │   │   ├── desktop-version-realmobile.css
│       │   │   ├── estilos-tiny-portal.css.dtml
│       │   │   ├── fontawesome-free-5.15.3-web-all.min.css
│       │   │   ├── forms.css
│       │   │   ├── hide.js
│       │   │   ├── ie7.css
│       │   │   ├── ie8.css
│       │   │   ├── invisibles.css
│       │   │   ├── jquery.tools.dateinput.css
│       │   │   ├── jquery.tools.dateinput.js
│       │   │   ├── koala-config.json
│       │   │   ├── mediaqueries.css
│       │   │   ├── mediaqueries_IE.css
│       │   │   ├── mediaqueries_versionNoMobile.css_OLD
│       │   │   ├── member.css
│       │   │   ├── mobile.css
│       │   │   ├── navtree.css
│       │   │   ├── pie
│       │   │   │   ├── PIE.htc
│       │   │   │   ├── PIE.php
│       │   │   │   ├── PIE_IE678.js
│       │   │   │   ├── PIE_IE678_uncompressed.js
│       │   │   │   ├── PIE_IE9.js
│       │   │   │   ├── PIE_IE9_uncompressed.js
│       │   │   │   └── PIE_uncompressed.htc
│       │   │   ├── ploneCustom.css.dtml
│       │   │   ├── portlets.css
│       │   │   ├── print.css
│       │   │   ├── public.css.dtml
│       │   │   ├── rotarImagen90.js
│       │   │   ├── versionmovil.css
│       │   │   └── webfont.eot.css
│       │   └── portal_theme_zsql
│       │       ├── consultar_accesos_por_usuario_uid.zsql
│       │       ├── consultar_datos_conexion_campus.zsql
│       │       ├── consultar_elementos_no_leidos.zsql
│       │       ├── eliminar_datos_conexion_campus.zsql
│       │       ├── getArchivosAltaSeguridad.zsql
│       │       ├── insertar_acceso_estadistica.zsql
│       │       ├── insertar_acceso_seguridad.zsql
│       │       ├── insertar_datos_conexion_campus.zsql
│       │       ├── modificar_datos_conexion_campus.zsql
│       │       └── update_extension_archivo_alta_seguridad.zsql
│       ├── skins.zcml
│       ├── tests.py
│       ├── tool.gif
│       ├── utiles.py
│       └── version.txt
├── portal.theme-configure.zcml
├── setup.cfg
└── setup.py


``` 
