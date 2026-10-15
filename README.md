# Mi TV — versión web

Reproductor de TV en vivo (IPTV) que funciona en el navegador. Es un solo archivo HTML: no hay que instalar nada ni compilar.

## Cómo usarla

1. Abre `index.html` con doble clic (Chrome, Edge o Firefox).
2. Elige una fuente en el selector de arriba.
3. Toca un canal para verlo.

| Acción | Cómo |
|---|---|
| Cambiar de canal | Botones ⏮ ⏭ o flechas **↑ ↓** del teclado |
| Cerrar el reproductor | Botón ✕ o tecla **Esc** |
| Buscar | Escribe en "Buscar canal…" |
| Filtrar por categoría | Menú de la izquierda (arriba en el celular) |
| Favoritos | Toca la ★ de un canal; aparecen en "★ Favoritos" |

## Fuentes de canales

| Fuente | De dónde sale |
|---|---|
| Países, idioma y temas | Listas públicas de [iptv-org](https://github.com/iptv-org/iptv): canales en abierto que las propias cadenas transmiten gratis por internet |
| Streams de prueba | Videos de demostración de Apple, Mux, Akamai y Unified Streaming. Sirven para comprobar que el reproductor funciona |
| Mi lista M3U | Tu propia lista: pega una URL y pulsa **Cargar**, o elige un archivo `.m3u` con **Archivo…** |

## Cómo funciona por dentro

Todo está en [index.html](index.html), en tres partes:

1. **Estilos (`<style>`)**: tema oscuro. Los colores están en variables al inicio (`--accent` es el rojo).
2. **Estructura (HTML)**: barra superior, menú de categorías, reproductor y cuadrícula de canales.
3. **Lógica (`<script>`)**:
   - `parseM3U(text)` lee la lista. Cada canal en un `.m3u` son dos líneas:
     ```
     #EXTINF:-1 tvg-logo="logo.png" group-title="Noticias",Nombre del canal
     https://servidor/stream.m3u8
     ```
     De la primera saca el nombre, el logo y la categoría; la segunda es el enlace del video.
   - `loadUrl(url)` descarga la lista con `fetch`.
   - `renderGroups()` y `render()` dibujan las categorías y las tarjetas según la búsqueda y el filtro.
   - `play(i)` reproduce un canal. Para HLS (`.m3u8`, el formato de casi todos los canales) usa la librería [hls.js](https://github.com/video-dev/hls.js), que se carga desde un CDN. Los formatos que el navegador entiende solo (como `.mp4`) se reproducen sin ella.
   - Los favoritos, la última fuente y tu URL propia se guardan en el `localStorage` del navegador.

## Limitaciones

- **Canales caídos:** las listas de iptv-org son comunitarias y algunos canales dejan de transmitir. Aparecen como "No disponible"; prueba con otro.
- **Bloqueo CORS:** algunos servidores no permiten que una página web reproduzca su video, aunque en una app sí funcionen. Es una regla de seguridad de los navegadores y no se puede saltar desde la página. La app Android no tiene este problema.
- **Listas propias:** si una URL tuya no carga por el mismo bloqueo, descarga el `.m3u` y ábrelo con **Archivo…**.
- **Solo TV en vivo:** la guía de programación, Chromecast y la sección de películas y series solo están en la app Android (`../iptv-android`).
