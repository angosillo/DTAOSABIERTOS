Propuesta 1: Datos Abiertos
Centinela Ambiental CyL: Tu Dashboard de Calidad del Aire y Riesgos Naturales
Descripción
Una aplicación web interactiva y accesible que permita a cualquier ciudadano de Castilla y León consultar de forma visual la calidad del aire en su localidad. El proyecto se centrará en la visualización de datos en tiempo real y la creación de un sistema de alertas simulado en el navegador.

Producto Mínimo Viable (MVP) - (Requisitos Obligatorios)
El alumnado deberá entregar una aplicación web funcional que cumpla con los siguientes puntos:
1.	Frontend Interactivo (JavaScript + Leaflet.js):
o	Crear un dashboard con un mapa de Castilla y León como elemento principal utilizando la librería Leaflet.js.
o	Obtener las coordenadas de las estaciones de medición del portal de datos abiertos y posicionarlas en el mapa mediante marcadores.
o	Al hacer clic en un marcador, se debe mostrar un pop-up con el nombre de la estación y los datos de contaminación más recientes.
2.	Visualización de Datos (JavaScript + Chart.js):
o	Implementar un espacio en la interfaz (un modal o un panel lateral) que se active al hacer clic en una estación.
o	Dentro de este espacio, se mostrará un gráfico de líneas generado con Chart.js que represente la evolución histórica (últimas 24 horas) de un contaminante principal (ej. PM10).
3.	Consumo de API (JavaScript + Fetch API):
o	La aplicación debe obtener todos los datos necesarios (ubicación de estaciones, mediciones en tiempo real) realizando llamadas asíncronas a las APIs del Portal de Datos Abiertos de la JCyL mediante el uso de la API fetch().
o	La interfaz debe cargarse de forma fluida, mostrando un indicador de "cargando" mientras se esperan los datos.
4.	Sistema de Alertas Simulado (JavaScript + localStorage):
o	Crear un formulario sencillo donde el usuario pueda seleccionar su municipio y establecer un umbral de alerta para un contaminante (ej. "Avisarme si PM10 supera 50 µg/m³").
o	Esta preferencia se guardará en el localStorage del navegador del usuario.
o	El código JavaScript de la aplicación comprobará, al recibir nuevos datos, si se cumple la condición de alerta guardada. Si es así, mostrará un aviso visual destacado dentro de la propia página (ej. un banner de color rojo en la parte superior).
5.	Tecnología PWA (Básica):
o	Añadir un fichero manifest.json a la aplicación, configurado con el nombre, iconos y colores de la app, para permitir que sea "instalable" en la pantalla de inicio de un dispositivo móvil.

Extensiones Opcionales (Mejoras para alumnos avanzados)
Estas funcionalidades no son obligatorias para superar el proyecto, pero permitirán al alumnado demostrar un nivel de competencia superior.
•	Extensión 1 (Backend Real de Alertas): Desarrollar un pequeño backend en PHP y una base de datos MySQL para gestionar un sistema de suscripciones real. El sistema deberá ser capaz de enviar un correo electrónico de alerta a los usuarios registrados cuando se cumplan las condiciones.
•	Extensión 2 (PWA Offline Avanzada): Programar un Service Worker en JavaScript para que la aplicación almacene en caché los últimos datos consultados, permitiendo una consulta básica de la información incluso cuando el dispositivo no tenga conexión a internet.
•	Extensión 3 (Geolocalización): Utilizar la API de geolocalización del navegador para detectar la ubicación del usuario, centrar el mapa automáticamente en su posición y destacar la estación de medición más cercana.

Temporalización y Planificación SCRUM para "Centinela Ambiental CyL"
Duración Total: 65 horas
Fase del Proyecto	Tiempo Asignado	Tareas / Casos de Uso (Backlog para Sprints)
Fase 1: Configuración y Diseño (Sprint 0)	10 horas	- Tarea 1 (Setup): Crear la estructura de carpetas del proyecto (HTML, CSS, JS). Inicializar el control de versiones con git init.
- Tarea 2 (Análisis de Datos): Investigar la API de Datos Abiertos de CyL. Identificar los endpoints exactos para obtener la lista de estaciones y los datos de mediciones.
- Tarea 3 (Diseño UI/UX): Crear un boceto/wireframe simple de la interfaz del dashboard: dónde irá el mapa, los gráficos y el formulario de alertas.
- Tarea 4 (Maquetación): Crear el archivo index.html con la estructura básica y el style.css con el diseño responsive inicial usando Flexbox o Grid.
Fase 2: Desarrollo del Mapa (Sprint 1)	15 horas	- CU 1 (Visualizar Mapa): Como usuario, quiero ver un mapa de Castilla y León centrado en la comunidad al cargar la página. (Implica integrar Leaflet.js).
- CU 2 (Pintar Estaciones): Como usuario, quiero ver marcadores en el mapa que representen la ubicación de todas las estaciones de medición de calidad del aire.
- CU 3 (Obtener Info Básica): Como usuario, al hacer clic en un marcador, quiero que aparezca un pop-up con el nombre de la estación y su municipio.
- CU 4 (Carga de Datos de Estaciones): La aplicación debe obtener la lista de estaciones y sus coordenadas de la API de JCyL usando fetch() al iniciarse.
Fase 3: Desarrollo de Gráficos y Datos (Sprint 2)	20 horas	- CU 5 (Mostrar Datos en Tiempo Real): Como usuario, al hacer clic en una estación, quiero ver los datos de contaminación más recientes en un panel lateral o modal.
- CU 6 (Visualizar Histórico): Como usuario, en ese mismo panel, quiero ver un gráfico de líneas (usando Chart.js) que muestre la evolución de las mediciones de PM10 de las últimas 24 horas.
- CU 7 (Carga Asíncrona de Mediciones): La aplicación debe realizar una nueva llamada fetch() para obtener los datos específicos de una estación solo cuando el usuario haga clic en ella.
- CU 8 (Gestión de Errores): La aplicación debe mostrar un mensaje de error amigable si la llamada a la API de JCyL falla.
Fase 4: Alertas, PWA y Pruebas (Sprint 3)	15 horas	- CU 9 (Configurar Alerta Local): Como usuario, quiero poder introducir mi municipio y un umbral de PM10 para que la aplicación me avise visualmente si se supera. (Implica usar localStorage).
- CU 10 (Mostrar Alerta Visual): La aplicación debe mostrar un banner de alerta visible si los datos recibidos superan el umbral que he configurado.
- CU 11 (Hacer la App Instalable): La aplicación debe tener un manifest.json para que pueda añadirla a la pantalla de inicio de mi móvil.
- Tarea 5 (Pruebas Funcionales): Verificar que todos los casos de uso (CU 1-10) funcionan correctamente. Probar la aplicación en diferentes navegadores y tamaños de pantalla.
Fase 5: Documentación y Defensa	5 horas (trabajo autónomo)	- Tarea 6 (Documentación): Redactar la memoria técnica del proyecto, explicando la arquitectura, las decisiones tomadas y las guías de uso.
- Tarea 7 (Preparación de Defensa): Crear la presentación (diapositivas) y ensayar la exposición pública del proyecto.

