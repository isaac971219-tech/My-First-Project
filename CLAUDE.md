# mi-primer-proyecto

Landing page personal de **Isaac Vergara Mejia** (Fitness Coach & Wellness Expert, Hamburgo, Alemania).

## Estructura

- `index.html` — página única (HTML + CSS inline, sin build ni dependencias). Secciones: hero, about, servicios, idiomas, contacto.
- `Isaacweb.jpeg` — foto de perfil usada en el hero.

## Diseño

Estilo brutalista oscuro/metálico: fondo con textura de concreto (ruido SVG + líneas de encofrado), acento verde lima vibrante, esquinas rectas, sombras duras (offset, sin blur). Tipografías: `Bebas Neue` (títulos), `Space Mono` (etiquetas/labels), `Inter` (cuerpo de texto) vía Google Fonts.

## Pagos

Botón "Book Session · €50" enlaza a un **Stripe Payment Link** (modo test/sandbox) — no requiere backend ni API keys en el código. Antes de producción, reemplazar por un Payment Link en modo Live desde el Stripe Dashboard.

**Regla:** nunca modificar el botón de pago de Stripe (`#btn-book`, su `href`, texto, precio o comportamiento) sin preguntar primero al usuario y esperar confirmación explícita antes de aplicar el cambio.

## Repositorio

Publicado en `github.com/isaac971219-tech/My-First-Project`. `.claude/` está excluido vía `.gitignore` (contiene credenciales locales de Claude Code).
