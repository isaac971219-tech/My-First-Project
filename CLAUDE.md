# mi-primer-proyecto

Landing page de **Isaac Vergara Mejia** (Fitness Coach & Wellness Expert, Hamburgo, Alemania).

## Estructura

- `index.html` — página única (HTML + CSS + JS inline, sin build ni dependencias). Secciones: hero, about, services, results, pricing (booking).
- `Isaac GYM.jpeg`, `Isaacweb.jpeg` — fotos de perfil (no referenciadas actualmente en `index.html`).
- `Back David laid.jpg` — foto de fondo full-bleed de la sección "About".
- `gym photo.jpg`, `brutalismus gym 1.jpg`, `brutalismus gym 2.jpg` — posters/fallback de los video-cards (hero, about, results respectivamente).
- `Isaac biceps.mp4`, `Video Lads.mp4`, `Video Pull ups.mp4` — clips personales de Isaac entrenando, usados como fondo en loop.
- `video thunder.mp4` — **no usado** en la página (excluido explícitamente a pedido del usuario).
- `Man in black.jpg` — no referenciado actualmente (quedó sin uso tras el rediseño a video).

## Diseño

Estilo editorial cinematográfico en blanco y negro atenuado: marco exterior gris oscuro suave (`#262626`) con un "lienzo" interior (`#1e1e1e`) tipo pantalla-dentro-de-pantalla. Tipografía `Inter` (Google Fonts) en todo el sitio. Botones siempre en píldora (blanco sólido o solo-borde). Sin iconos ni badges de colores — el peso visual lo llevan la tipografía y la fotografía/video.

**Color de fondos (fotos y videos):** NO usar `grayscale(100%)`. El filtro estándar del sitio es `saturate(0.6) contrast(1.05) brightness(0.85)` — deja pasar algo de color real pero atenuado, manteniendo la atmósfera oscura. Aplicarlo consistentemente a cualquier foto/video de fondo nuevo.

### Video de fondo (hero / about / results)

Tres secciones tienen su propio video en loop personal de Isaac, repartido así:
- **Hero** → `Video Pull ups.mp4` (espalda/pull-ups) — el video destacado, ver abajo
- **About** → `Isaac biceps.mp4`
- **Results** → `Video Lads.mp4`

Resolución nativa (verificada leyendo el box `tkhd` de cada MP4 — `Video Pull ups.mp4` es, de los tres, el de **menor** resolución):
- `Video Pull ups.mp4`: 464×832
- `Isaac biceps.mp4` / `Video Lads.mp4`: 576×1024

Cada uno vive dentro de un componente reutilizable `.video-card`: contenedor acotado (`max-width: 560px`, `aspect-ratio: 4/5`, `border-radius: 12px`) — **nunca full-bleed a pantalla completa**, para evitar que un clip de baja resolución se vea pixelado al estirarse. El `<video>` interior overscanea un poco su marco (`top:-8%; height:116%`) para dar espacio al parallax sin dejar huecos.

El video del Hero (`Video Pull ups.mp4`) lleva además la clase `.video-card--featured`, que sube `max-width` a `720px` para darle más protagonismo que a los otros dos — un aumento moderado (no mayor) precisamente porque es el clip de menor resolución nativa; subirlo más allá empezaría a notarse borroso.

**Parallax:** implementado en JS puro (`translate3d` actualizado con `requestAnimationFrame` en scroll) y aplicado **únicamente a los `.video-card`** (el selector del script es `document.querySelectorAll('.video-card')`) — deliberadamente NO se usa `background-attachment: fixed` (poco confiable en Safari/iOS). Respeta `prefers-reduced-motion`: si está activo, se desactiva el parallax y el autoplay, dejando solo el poster estático. La capa de fondo general (ver abajo) queda fuera de este selector a propósito, así que no tiene ningún parallax.

**Rendimiento:** cada video usa `IntersectionObserver` para reproducirse solo cuando su sección está en el viewport y pausarse al salir — evita tener 3 videos decodificando simultáneamente sin necesidad. Atributos `autoplay muted loop playsinline` en el HTML como base.

**Nota de verificación:** el entorno de automatización de navegador usado durante el desarrollo (Claude in Chrome) no logra decodificar video en absoluto — se confirmó probando incluso un video de referencia externo conocido (MDN), aislado del código del proyecto, que falló igual. Los 3 archivos de video se verificaron como MP4 válidos (H.264, moov atom al inicio, fast-start) vía inspección de bytes; el comportamiento real de autoplay/parallax debe confirmarse en un navegador normal (o en el sitio ya publicado en GitHub Pages).

### Capa de fondo general (textura de página, sin cortes)

Las mismas 3 fotos "gym" forman además una **única capa de fondo continua** (`.page-texture`, con tres `.page-texture-layer` dentro: `.pt-1`/`.pt-2`/`.pt-3`) que cubre todo el alto de `.canvas` desde el principio — detrás de absolutamente todo, incluidos los video-cards (`z-index: 0` contra el `z-index: 2` de nav/secciones/footer). Es intencionalmente **estática**: al no llevar la clase `.video-card`, el script de parallax nunca la toca.

Cada capa cubre ~44% del alto total, con un solapamiento generoso (~16%) entre capas consecutivas, y cada una tiene su propio `mask-image: linear-gradient(...)` que la desvanece a transparente en sus bordes — así una foto se disuelve suavemente en la siguiente en vez de cortar en seco. Un único overlay compartido (`.page-texture::after`, `rgba(30,30,30,0.78)`) va por encima de las tres para que la opacidad (~22% de imagen visible) se vea uniforme y no se oscurezca de más en las zonas de solape:

- **`.pt-1`** (arriba, 0%–44%) → `brutalismus gym 2.jpg`
- **`.pt-2`** (medio, 28%–72%) → `gym photo.jpg`
- **`.pt-3`** (abajo, 56%–100%) → `brutalismus gym 1.jpg`

Mismo filtro `saturate(0.6) contrast(1.05) brightness(0.85)` que el resto del sitio.

## Pagos

Botón "Book Session · €50" enlaza a un **Stripe Payment Link** (modo test/sandbox) — no requiere backend ni API keys en el código. Antes de producción, reemplazar por un Payment Link en modo Live desde el Stripe Dashboard.

**Regla:** nunca modificar el botón de pago de Stripe (`#btn-book`, `#btn-book-hero`, su `href`, texto, precio o comportamiento) sin preguntar primero al usuario y esperar confirmación explícita antes de aplicar el cambio.

## Repositorio

Publicado en `github.com/isaac971219-tech/My-First-Project`, con GitHub Pages activo en `https://isaac971219-tech.github.io/My-First-Project/`. `.claude/` está excluido vía `.gitignore` (contiene credenciales locales de Claude Code).
