# 🖥️ 06 · El visor (frontend)

`scaffold/frontend/` del repositorio oficial: **React 18 + Vite 6 + TypeScript 5**, con **Aladin Lite 3.8.2** para el cielo. Unas 12.500 líneas de TypeScript y 960 de CSS (medido en `main`, `5b599b9`).

## 🧭 Cómo navega una persona

```
Portada ──► Vista simple ──► Modo ciencia (el Visor completo)
   │            (una imagen grande, fechas, barra «Luz»)    │
   ├──► Modo Misión (5 niveles guiados, cada uno es un enlace)
   ├──► Tutorial guiado por Spherix (mascota)
   └──► Descargas (instaladores y guía de iPhone)
```

El menú ☰ del Modo ciencia ofrece Inicio, Visor, Modo Misión, Ver tutorial, Descargas y Repositorio. Hay selector de idioma español/inglés.

## 🗺️ Mapa de carpetas (`src/`)

| Carpeta | Líneas | Qué hace |
|---|---|---|
| `sky/` | 3.133 | El cielo en Aladin: cubo HiPS y deslizador de λ, parpadeo SPHEREx contra 2MASS/AllWISE, y capas de catálogos (Gaia, CatWISE, SIMBAD, trayectoria, marca del objetivo) |
| `explorador/` | 1.849 | El Explorador de imágenes por fecha y banda: resta, color, *apilar visitas*, *limpiar estrellas*, *Resta A − B* |
| `prisma/` | 1.731 | Vista multibanda: «Ojo infrarrojo» (color traducido), huella espectral, qué cambió entre pasadas, tipo probable del astro, fondo del cielo |
| `content/` | 1.348 | Todos los textos y datos de contenido (campos de prueba, Modo Misión, tutorial, Spherix) |
| `apuntar/` | 1.035 | Apuntar a cualquier parte: coordenadas en grados o sexagesimal, «¿Qué hay aquí?» (SIMBAD), zona al azar, espectro de un punto |
| `pages/` | 904 | Pantallas: portada con cielo animado, Visor, Descargas |
| `simple/` | 620 | La vista simple sin «Modo ciencia» y el interruptor que pasa al Visor completo |
| `spherix/` | 428 | La mascota: chat flotante determinista (sin modelo de lenguaje) |
| `mision/` | 363 | El Modo Misión: comparador «a lo Tombaugh» con fotos reales, glosario con términos al pasar el mouse |
| `tutorial/` | 281 | Recorrido guiado que ilumina cada sección del visor |
| `comentarios/` | 220 | Comentarios del público al final del Visor, sin servidor |
| `blink/` | 168 | Parpadeo por visitas con `/api/frames` |
| `huella/` | 126 | Resultados de la huella espectral precalculados |
| `state/` | 121 | Estado ↔ URL, idioma |

## 💎 Piezas que vale la pena entender

- **Estado en la URL** (`state/url.ts`): `ra`, `dec`, `size` (máximo 0,5°), `band` (D1–D6), `mision`, `tutorial`, `ciencia`, `vista`. Cada nivel del Modo Misión y cada vista es un enlace compartible. Un enlace viejo con `size=1` se recorta al máximo.
- **Datos del explorador** (`explorador/useDatosSpherex.ts`): carga la rejilla de visitas por `/api/grid` y los fotogramas como float32 crudos, que `render.ts` pinta en el navegador.
- **Posición propagada** (`sky/propagacion.ts`): port de `posicion_en` del backend; movimiento propio lineal, sin paralaje.
- **Presets** (`explorador/presets.ts`): WISE 0855 y eps Ind B abren con banda 5 y la **mediana de 10 exposiciones por visita**, porque con una sola no se ve el movimiento.
- **Capas de catálogos** (`sky/capas/`): consultan SIMBAD (CDS primero, espejo de Harvard de respaldo), Gaia y CatWISE directo desde el navegador, con estado visible y botón «Reintentar» si no responden.
- **Accesibilidad:** respeta `prefers-reduced-motion` (el parpadeo automático se apaga y se ofrece «Alternar época»), textos de ayuda, y botones de al menos 44 px en móvil.

## 📲 PWA

El visor es una aplicación web instalable: `manifest.webmanifest`, íconos (192, 512 y *maskable*) y un `sw.js` con caché del *shell* (`spherex-shell-v1`). Las llamadas a `/api`, `/proto-api` y `/health` **nunca se cachean**: los datos siempre vienen frescos. En iPhone/iPad es la única vía de instalación (ver [07](07_APPS_NATIVAS_Y_RELEASES.md)).

## ⚡ Datos precalculados (`public/precalc/`)

Archivos estáticos que el visor lee primero para que los cinco objetos de prueba carguen al instante: `espectro/`, `explorador/` (WISE 0855 y eps Ind B), `huella/`, `mision/` y `simple/`.

## ▶️ Cómo correrlo

```bash
cd scaffold/frontend
npm ci
npm run dev        # http://localhost:5173, con proxy a la API local (8000) y al prototipo
npm run build      # tsc -b && vite build
npm run lint       # tsc --noEmit
```

No hay pruebas automatizadas del frontend; el CI solo compila (`npm run build`). `tsc --noEmit` termina sin errores.
