# Descripción general

Plone es un sistema de administración de contenido (CMS) conocido por su interfaz fácil de usar y sus sólidas funciones de seguridad. Con Plone, incluso los usuarios sin conocimientos técnicos pueden crear y administrar fácilmente el contenido de un sitio web público o una intranet usando solo un navegador web. La interfaz intuitiva y el completo conjunto de funciones de Plone lo convierten en una opción popular para empresas, gobiernos, universidades y cualquier organización que necesite un CMS confiable y fácil de usar.

Plone tiene una larga historia y cuenta con la confianza de usuarios de todo el mundo desde su lanzamiento inicial el 4 de octubre de 2001. A lo largo de los años, Plone ha experimentado muchas mejoras y mejoras, lo que lo convierte en un CMS altamente maduro y estable. Además, Plone cuenta con el respaldo de una sólida comunidad de usuarios y desarrolladores que contribuyen a su éxito continuo.

Plone tiene la madurez, estabilidad y confiabilidad de una aplicación mantenida por desarrolladores de código abierto con décadas de experiencia, mientras evoluciona y se adapta continuamente a la tecnología moderna.

Se pueden realizar muchas personalizaciones a través de la web, como crear tipos de contenido, temas, flujos de trabajo y mucho más. Un flujo de trabajo de desarrollo basado en un sistema de archivos completo es posible y se recomienda para el trabajo en equipo y la implementación, respaldado por repositorios de código fuente. Plone se puede ampliar y utilizar como un marco sobre el que construir soluciones personalizadas similares a CMS.

Plone funciona como un:

* Un CMS HTML renderizado del lado del servidor con todas las funciones.
* Un frontend basado en React para la edición y visualización del contenido, con un backend con una API REST.
* Un servidor Headless con una API REST, que permite a los desarrolladores construir su propia interfaz personalizada en la tecnología que prefieran.

## Principales beneficios

La seguridad está integrada en la arquitectura de Plone desde cero. Plone ofrece un control de permisos detallado sobre el contenido y las acciones.

Plone es fácil de configurar en comparación con otros CMS en su categoría, extremadamente flexible y le brinda un sistema para administrar contenido web que es ideal para grupos de proyectos, comunidades, sitios web, extranets e intranets.

* Plone empodera a los editores de contenido y desarrolladores de aplicaciones web. El equipo de Plone incluye expertos en usabilidad que han hecho que Plone sea fácil y atractivo para que los administradores de contenido agreguen, actualicen y mantengan contenido.
* Plone es internacional. La interfaz de Plone tiene más de 35 traducciones y existen herramientas para administrar contenido multilingüe.
* Plone sigue estándares y es inclusivo. Plone sigue cuidadosamente los estándares de usabilidad y accesibilidad. Plone cumple con WCAG 2.1 nivel AA y apunta a ATAG 2.0 nivel AA.
* Plone es de código abierto. Plone tiene la licencia GNU General Public License, la misma licencia utilizada por Linux. Esto le otorga el derecho de usar Plone sin pagar una licencia y de mejorar el producto.
* Plone es compatible. Hay más de doscientos desarrolladores activos en el equipo de desarrollo de Plone en todo el mundo y una multitud de empresas que se especializan en el desarrollo y soporte de Plone.
* Plone es extensible. Hay una multitud de productos complementarios para que Plone agregue nuevas funciones y tipos de contenido. Además, Plone se puede programar utilizando soluciones estándar web y lenguajes de código abierto.
* Plone es tecnológicamente neutral. Plone puede interoperar con la mayoría de los sistemas de bases de datos relacionales, tanto de código abierto como comerciales, y se ejecuta en muchas plataformas, incluidas Linux, Windows, macOS y BSD.

## Descripción a alto nivel para desarrolladores

Plone es una plataforma de administración de contenido con su backend escrito en Python. Está construido sobre el servidor de aplicaciones web y el sistema de desarrollo de código abierto Zope. Plone hace uso de la arquitectura de componentes Zope conectable (ZCA) para proporcionar un sistema altamente modular y extensible. A lo largo de su historia, Plone ha utilizado la representación del lado del servidor para generar contenido basado en HTML, con funciones avanzadas de administración de recursos para agregar y agrupar CSS y JavaScript. Además, el uso de Plone de una arquitectura de componentes facilita la ampliación y la personalización, lo que permite a los usuarios crear sitios web únicos y ricos en funciones que se adaptan a sus necesidades específicas.

Con el lanzamiento de Plone 6, ahora tiene la opción de elegir entre dos configuraciones compatibles listas para usar diferentes al configurar un nuevo sitio web de Plone. El servidor back-end basado en Python en Plone todavía se puede usar solo para representar el contenido del lado del servidor y entregar HTML al navegador, una configuración a la que se hace referencia en la documentación de Plone como "IU clásica". Esta configuración ha sido compatible con Plone desde su lanzamiento inicial y todavía está disponible en la última versión de la plataforma. Para la implementación basada en contenedores, solo se requiere la imagen de ==plone-backend==. Puede usarse como imagen base, agregando personalizaciones, para hacer una imagen derivada.

La configuración predeterminada y recomendada para nuevos sitios web en Plone es la nueva interfaz de JavaScript basada en React llamada "Volto". Para esta configuración, aún necesita ejecutar el servidor back-end basado en Python, así como habilitar la API REST y actualizar el perfil de configuración. Esta configuración y perfil se aplican automáticamente cuando selecciona la opción ``Crear sitio Plone`` en el formulario de creación de sitio web Plone. Además, un servidor de interfaz de usuario basado en NodeJS independiente servirá los recursos de interfaz de JavaScript y proporcionará SSR con hydration. Para implementar esta configuración usando contenedores, necesitará la imagen ==plone-frontend== para el servidor frontend.

A partir de Plone 6, ahora admitimos dos pilas de lenguajes de programación, una para Python y otra para JavaScript. La documentación se ha reescrito, pero para esta primera versión encontrará algunas repeticiones de conceptos en la estructura de la documentación. Consulte, por ejemplo, la configuración de desarrollo y la información sobre las opciones de implementación. Tomará algún tiempo hasta que encontremos y podamos implementar la mejor estructura para explicar estas nuevas posibilidades y la expansión de las capacidades de Plone.

## Despliegue

Para ejecutar un sitio web público de Plone en producción, también deberá configurar y ejecutar un proxy inverso (o ingress), organizar certificados SSL (ya sea de Let's Encrypt o manualmente), garantizar la persistencia de la base de datos de contenido y organizar copias de seguridad. Este es del dominio de los administradores de sistemas y los profesionales de operaciones modernas de desarrollo. Nuestra documentación contiene ejemplos de configuración para estos servicios, pero requiere que el lector tenga alguna experiencia genérica y conocimiento de estas áreas.

## Está bien conocer / Qué saber

Uno de los beneficios clave de la nueva interfaz basada en React para Plone 6 es que ahora puede personalizar y aplicar temas a Plone ampliamente utilizando HTML, CSS y JavaScript utilizando tecnologías de interfaz actualizadas sin tener que configurar un entorno de desarrollo de Python local. El backend de Plone se puede ejecutar en una máquina de desarrollador local en un contenedor.

Se requiere una familiaridad básica con la programación en Python y la administración de módulos y paquetes de Python usando ``virtualenv`` y ``pip`` para trabajar en el código de ``back-end``. Usamos ``virtualenv`` y ``mxdev`` para administrar la instalación fuente de paquetes en Plone 6.

Del mismo modo, para desarrollar para la nueva interfaz React, debe tener algo de experiencia en la configuración de NodeJS, usar una herramienta como NVM (Node Version Manager) para aislar su configuración y estar familiarizado con **Yarn** y **React**.

Si está buscando más material de estudio sobre estas tecnologías más allá de la documentación, vea y siga uno o más [entrenamientos de Plone](https://training.plone.org/). Nuestras capacitaciones son más detalladas y contienen aclaraciones y ejemplos adicionales.
