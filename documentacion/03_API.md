# 🔗 03 · La API: lo que dice el contrato y lo que hace el código

El backend es FastAPI (`scaffold/backend/app/main.py` del repositorio oficial, versión 0.4.2). El **contrato** vive en `docs/03_ARQUITECTURA.md` §3 y es «lo único que frontend y backend pueden dar por cierto». Este documento resume los dos y marca dónde no coinciden (los hallazgos de la [auditoría](09_RIESGOS_Y_AUDITORIA.md) con su número de issue).

> [!NOTE]
> Convenciones del contrato: coordenadas en **grados decimales** (ICRS), tamaños en **grados**, fechas en **ISO 8601 UTC**.

## 📍 Endpoints que existen en el código

| Ruta | Para qué | Estado frente al contrato |
|---|---|---|
| `GET /health` y `GET /api/health` | Comprobación de vida: `{ok, version, irsa_reachable, cache_entries}` | `irsa_reachable` siempre es `null`; el contrato promete `true/false` ([#79](https://github.com/mauriciovalenzuela742/SPHEREX/issues/79)) |
| `GET /api/epochs` | Qué observaciones existen para una posición. Barato: no baja imágenes | Implementado. Acepta `from`/`to` |
| `GET /api/grid` | Alias de `/api/epochs` | **Fuera del contrato**, pero el Explorador del visor lo usa ([#75](https://github.com/mauriciovalenzuela742/SPHEREX/issues/75)) |
| `GET /api/frames` | El corazón: un fotograma por visita (o por barrido) ya renderizado | Implementado; difiere en parámetros ([#73](https://github.com/mauriciovalenzuela742/SPHEREX/issues/73)) |
| `GET /api/frame` | Alias de `/api/frames` | **Fuera del contrato** ([#75](https://github.com/mauriciovalenzuela742/SPHEREX/issues/75)) |
| `GET /api/spectrum` | Espectro de una posición, al vuelo | Implementado |
| `GET /api/demo` | Lista los campos precalculados (los 5 de la demo) | Extra, documentado en el contrato |

> [!WARNING]
> **No existe** `GET /api/resolve?q=` (resolución de nombres por SIMBAD) aunque el contrato lo define; el visor consulta SIMBAD directo desde el navegador ([#72](https://github.com/mauriciovalenzuela742/SPHEREX/issues/72)).

## 🗓️ `/api/epochs`

Parámetros: `ra` (0–360), `dec` (−90–90), `size` (defecto 0,1; hasta 1°), `collections` (defecto `spherex_dr1,spherex_qr3`), `from`, `to`.

Respuesta: `n_epochs`, `time_range`, `epochs[]` (cada una con `id`, `t`, `t_mjd`, `band`, `detector`, `collection`, `access_url`, `pixel_scale_arcsec`), más las estructuras que el Explorador necesita: `bands`, `visits` (por banda), `columns` (una por fecha) y `source` (`vivo` o `precalculado`).

La banda se deduce del último dígito del nombre del detector; el propio código lo declara «NO verificado contra IRSA» y una época sin banda se descarta en silencio ([#76](https://github.com/mauriciovalenzuela742/SPHEREX/issues/76)).

## 🎞️ `/api/frames`

| Parámetro | Valores | Defecto |
|---|---|---|
| `ra`, `dec` | grados (`ra` < 360) | — |
| `size` | 0,005 < size ≤ 0,5 (más da 422) | 0,1 |
| `band` | 1–6 | 4 |
| `stretch` | `asinh` o `linear` | `asinh` |
| `mode` | `zodi_subtracted`, `raw`, `diff_first`, `diff_prev`, `coadd`, `coadd_diff` | `zodi_subtracted` |
| `from`, `to` | fechas ISO | — |
| `per_visit` | 1–10 exposiciones combinadas por visita (mediana) | 1 |
| `budget_s` | tiempo máximo en segundos (≤ 55) | 8 |
| `epochs` | lista de `epoch_id` para completar lo `pending` | — |
| `vmin`, `vmax` | repetir la escala de la primera respuesta | — |
| `fast` | lector por rangos (rápido, sin FLAGS) o servicio de recortes | `true` |
| `max_per_sweep` | exposiciones por barrido en `coadd` | 40 |

Respuesta: `frames[]` (cada uno con `t`, `epoch_id`, `wavelength_um`, `bandwidth_um`, `png` como *data URI*, `flagged_fraction` —puede ser `null` con el lector por rangos—, `vmin`/`vmax`, `n_stacked`, `method`), `pending`, `warnings`, `unit` (`visita` o `barrido`), `elapsed_s` y `source`.

**Lo que el contrato documenta y el código no acepta** ([#73](https://github.com/mauriciovalenzuela742/SPHEREX/issues/73)): `band=all`, `cmap`, y los estiramientos `sqrt`, `log` y `zscale`. Un cliente que pida `stretch=log` recibe 422.

**Suma por barrido (`coadd`, `coadd_diff`):** todas las exposiciones de un barrido se reproyectan a la rejilla común y se suman con peso 1/VARIANCE: Σ(d/σ²)/Σ(1/σ²). `coadd_diff` resta el primer barrido.

## 🌈 `/api/spectrum`

Parámetros: `ra`, `dec`, `aperture` (radio en ″, 2–60, defecto 12), `from`, `to`, `bands` (lista, defecto 1–6), `max_points` (6–600), `budget_s`, `epochs`, y para objetos con movimiento propio `pmra`, `pmdec`, `epoch`.

Un punto por exposición de nivel 2: fotometría de apertura en píxeles nativos, fondo = mediana de un anillo, error con VARIANCE, longitud de onda en el píxel del objeto, flujo en mJy. El campo **`caveat` es obligatorio**: avisa que son valores indicativos, no fotometría calibrada.

## ⏱️ Caché, tiempo y errores

- Los 5 campos de prueba salen de `data/demo/` sin tocar IRSA (`source: "precalculado"`), solo con los parámetros por defecto de la demo.
- IRSA devuelve 503 transitorios: el backend reintenta con espera y, si no hay caché, responde 503 con un mensaje claro.
- Con `OFFLINE=1` nunca se toca internet y lo que no está en caché es un error explícito.

## 🧭 URL de la aplicación

El contrato (§4) define `?ra=&dec=&size=&band=&fps=&stretch=&mode=` y `?target=` para que copiar la barra de direcciones comparta el estado exacto del visor. Hoy el visor lee y escribe `ra`, `dec`, `size`, `band`, `mision`, `tutorial`, `ciencia` y `vista`; **no** `target`, `fps`, `stretch` ni `mode` ([#74](https://github.com/mauriciovalenzuela742/SPHEREX/issues/74)). Mi vista [«Descargas»](https://spherex-planet-x-tau.vercel.app/?vista=descargas) usa `?vista=descargas`.
