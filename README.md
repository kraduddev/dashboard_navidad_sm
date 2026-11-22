# Navidad en Santa Marta · 2026/27

Web estática e interactiva para consultar el programa de Navidad 2026/27 de **Santa Marta de Tormes (Salamanca)**, del **4 de diciembre de 2026 al 6 de enero de 2027**. Presenta las actividades mediante una estética de cartel navideño vintage, con tarjetas en forma de entrada.

> **Estado:** prototipo funcional, basado en el cartel municipal y en los JSON preparados para el proyecto del bot de Telegram. No es una publicación oficial del Ayuntamiento ni dispone de actualización automática.

## Estado actual

| Dato | Valor |
| --- | ---: |
| Registros de eventos | 157 |
| Citas especiales | 21 |
| Registros de visitas recurrentes | 136 |
| Fechas cubiertas | 34 días |
| Ubicaciones incluidas en los datos | 14 |
| Ubicaciones con citas especiales | 6 |

Los **136 registros recurrentes** representan cuatro actividades o instalaciones repetidas diariamente: **Belén navideño**, **Iluminación navideña**, **Isla de la Navidad** y **Santa Marta Villa Navideña**. No equivalen a 136 actividades distintas.

## Funcionalidades implementadas

- **Portada** con resumen de temporada, accesos rápidos y próximas citas; antes del inicio muestra los primeros eventos y después del cierre muestra las últimas citas.
- **Tarjetas de actividades** con fecha, hora, categorías, descripción breve y ubicación. Se pueden abrir con ratón, pulsación o teclado.
- **Calendario de diciembre de 2026 y enero de 2027**, con indicadores de cuántas citas especiales hay cada día y detalle seleccionable.
- **Bloque de iluminación y visitas** con las cuatro actividades recurrentes.
- **«Dónde está la Navidad»**, catálogo visual de ubicaciones con enlaces para filtrar por lugar y abrir una búsqueda en Google Maps. **No es un mapa geográfico ni calcula rutas.**
- **Explorador de eventos** con búsqueda, filtros por categoría, ubicación y tipo, chips rápidos («Peques y familia», «Espectáculos», «Reyes» y «Visitas»), paginación con «Ver más» y restablecimiento de filtros.
- **Ficha emergente de evento** con fecha, horario, categorías, descripción, ubicación y, según el caso, acciones para:
  - Abrir el lugar en **Google Maps**.
  - Crear un evento en **Google Calendar**.
  - Descargar un archivo **`.ics`**.
  - **Compartir** mediante las capacidades del dispositivo o copiar la información.
- **Enlaces directos a eventos** con fragmentos del tipo `#evt-2026-12-05-correfoc-ragnarok` cuando la página está publicada en una URL compartible.
- **Exportación del conjunto de eventos a JSON** desde el pie de página, en contextos admitidos.
- **Diseño responsive**, **modo oscuro** según preferencias del sistema, controles de teclado, uso de `<dialog>` y soporte para reducir animaciones.

## Tecnología y arquitectura

- **HTML5, CSS3 y JavaScript vanilla**; sin frameworks, compilación ni servidor de aplicación.
- Un solo archivo HTML contiene la interfaz, estilos, lógica y datos incorporados en la constante `DATA`.
- Fuentes externas desde **Google Fonts**: Fraunces, Space Mono y Caveat. Si no hay conexión, se utilizan alternativas configuradas en CSS.
- Integración mediante enlaces con Google Maps y Google Calendar; no se usan claves API.
- Sin base de datos, autenticación ni panel de administración.

### Estructura disponible

La última versión revisada se encuentra en:

```text
Navidad en Santa Marta · 2026_27(1).html
```

En los archivos de trabajo también existen los conjuntos de datos de referencia:

```text
eventos_navidad_santa_marta_2026_2027_completo_lugares_corregidos.json
places_navidad_2026_2027_actualizado.json
```

**Importante:** el HTML actual **no carga esos archivos en tiempo de ejecución**. Los datos están incrustados en `const DATA = { events: [...], places: [...] }`. Modificar un JSON externo por sí solo no actualiza la web: hay que regenerar o editar el bloque `DATA` del HTML.

## Puesta en marcha

1. Abrir el archivo HTML con un navegador moderno.
2. Para publicarlo, subirlo como archivo `index.html` a un hosting estático (o adaptar el nombre a la configuración del alojamiento).
3. No se necesita instalar dependencias ni iniciar un backend.

Las búsquedas y el calendario funcionan localmente. Los enlaces de Maps, Google Calendar y las fuentes externas requieren conexión. Algunas funciones de compartir o portapapeles pueden estar restringidas al abrir la página con `file://` o dentro de un iframe.

## Datos de eventos y lugares

El modelo de evento sigue este formato:

```json
{
  "Id": "evt-2026-12-05-correfoc-ragnarok",
  "Title": "Espectáculo Correfoc «Ragnarok»",
  "Start": "2026-12-05T19:00:00+01:00",
  "End": null,
  "PlaceId": "avda_nieves",
  "Category": ["espectaculo", "pasacalles", "aire_libre"],
  "Summary": "A cargo de Kull D’Sac. Salida desde la avenida de las Nieves y recorrido por el municipio."
}
```

Los lugares utilizan `Id`, `Name`, `SearchString` y `Features`. `PlaceId` enlaza el evento con el lugar; puede ser `null` para actividades distribuidas por el municipio, como la iluminación navideña.

La web distingue las visitas recurrentes de las citas especiales por la categoría `visita`. En el explorador, las visitas diarias se **agrupan por título** para no mostrar 34 tarjetas idénticas por cada instalación. Los 157 registros originales permanecen incrustados y disponibles para exportación.

## Limitaciones y aspectos pendientes

1. **Ubicaciones sin programación confirmada:** «Dónde está la Navidad» muestra las 14 entradas de `places`, incluso algunas sin eventos ni elementos decorativos acreditados para esta campaña. Conviene mostrar al público solo lugares con contenido confirmado o distinguir los históricos.
2. **Resultados de ubicaciones vacías:** al filtrar un lugar sin actividades, la interfaz puede mostrar un mensaje de cero citas junto con iluminación genérica del municipio. Este comportamiento debería simplificarse.
3. **Duraciones no publicadas:** los eventos con `End: null` se crean en Google Calendar con **una hora de duración provisional**. No debe interpretarse como duración oficial. En `.ics`, el evento no incluye `DTEND` si se desconoce.
4. **Mapas:** los enlaces abren búsquedas por `SearchString`; no hay mapa interactivo interno ni coordenadas verificadas para cada punto.
5. **Actualización manual:** cualquier modificación de horarios, ubicaciones o cancelaciones exige actualizar los datos incrustados en el HTML.
6. **Visitas repetidas:** la ficha de una visita recurrente utiliza como referencia uno de los registros diarios; la fecha aplicable al usuario debe tratarse con cuidado al compartir o añadir al calendario.
7. **Dependencias externas:** las tipografías y los servicios enlazados requieren acceso a Internet.

## Siguientes pasos recomendados

- Corregir el listado de ubicaciones y el estado vacío de sus filtros.
- Señalizar explícitamente las horas de fin estimadas en Google Calendar.
- Incorporar un mapa geográfico real solo para puntos correctamente localizados.
- Separar datos (`events.json`, `places.json`) de la presentación y definir un proceso de actualización reutilizable.
- Añadir pruebas automáticas para IDs únicos, fechas, horarios y referencias `PlaceId`.
- Valorar enlaces entre esta agenda web y el **bot de fiestas de Telegram**. **Esta integración todavía no está implementada.**

## Fuente y alcance

Contenido elaborado a partir del programa navideño 2026/27 de Santa Marta de Tormes y de los JSON derivados en este proyecto. Antes de su publicación pública conviene contrastar la programación con las comunicaciones municipales más recientes.
