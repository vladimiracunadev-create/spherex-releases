# 05 · El backend

`scaffold/backend/` del repositorio oficial: **Python 3.11 + FastAPI**, unas 1.830 líneas de aplicación y 310 de pruebas (medido en `main`, `5b599b9`).

## Mapa de módulos

```
scaffold/backend/app/
├── main.py                 rutas de la API, CORS, versión (202 líneas)
├── demo.py                 sirve los 5 campos precalculados sin tocar IRSA
├── irsa/                   acceso a los datos (rol D1)
│   ├── config.py           rutas y constantes (colecciones, paralelismo, reintentos)
│   ├── sia2.py             búsqueda de épocas por SIA2 (solo metadatos)
│   ├── cabeceras.py        cabecera de la extensión IMAGE por HTTP Range (~35 KB)
│   ├── cutouts.py          recortes FITS de IRSA, máx. 4 en paralelo, con caché
│   ├── descarga.py         GET con reintentos (IRSA da 503 transitorios)
│   ├── cache.py            caché en disco con escritura atómica
│   ├── posicion.py         posición propagada por movimiento propio, campos de prueba
│   ├── precalcular_demo.py genera el paquete data/demo/ (corre una vez, con red)
│   └── precalentar.py      precalienta la caché con los 5 campos
├── frames/                 fotogramas (rol D2)
│   ├── servicio.py         /api/frames: una visita por fotograma, presupuesto de tiempo
│   ├── rangos.py           lector rápido por rangos HTTP (solo los píxeles necesarios)
│   ├── procesar.py         reproyección a rejilla común, suma ponderada 1/VARIANCE, restas
│   └── render.py           fotograma numérico → PNG (data URI), escala común por banda
└── spectrum/               espectro al vuelo (rol D2)
    ├── servicio.py         /api/spectrum: un punto por exposición
    └── fotometria.py       fotometría de apertura con fondo en anillo
```

Los tres módulos más grandes son `frames/servicio.py` (331 líneas), `frames/rangos.py` (272) y `frames/procesar.py` (266).

## Cómo se arma un fotograma

1. **Qué hay:** `sia2.buscar_epocas` pregunta a IRSA qué archivos cubren la posición, en `spherex_dr1` y `spherex_qr3`.
2. **Qué longitud de onda:** `cabeceras` (o `rangos.leer_wcs_wave`) lee la cabecera y la tabla `WCS-WAVE` sin bajar el archivo.
3. **Los píxeles:** `rangos.leer` pide por HTTP Range solo el recorte necesario de IMAGE, y la varianza si hace falta. Alternativa: `cutouts` con el servicio de recortes.
4. **Alinear:** `procesar` reproyecta a la rejilla TAN común, norte arriba (el detector gira ~180° entre barridos).
5. **Combinar:** mediana de `per_visit` exposiciones, o suma por barrido con peso 1/VARIANCE (`coadd`).
6. **Pintar:** `render` aplica estiramiento `asinh` o lineal con **escala común a toda la banda** (para que el brillo no «salte» entre fechas) y devuelve el PNG como *data URI*.
7. **Respuesta parcial:** `servicio.fotogramas` procesa primero la primera y la última visita (la base temporal es lo que muestra el movimiento). Si se acaba `budget_s`, devuelve lo listo y en `pending` los `epoch_id` que faltan.

## Pruebas

46 pruebas con `pytest`, todas **sin red** (corrieron el 2026-11-15, Python 3.12.9 con las versiones fijadas de `requirements.txt`):

| Archivo | Qué cubre |
|---|---|
| `test_demo.py` | Los 5 campos sin red; aviso al centrar en la posición J2000; falla clara sin caché; `/api/demo` |
| `test_frames_spectrum.py` | Suma ponderada por 1/VARIANCE; separación de barridos a más de 60 días; fotometría que recupera el flujo de una gaussiana; rechazo de apertura saturada; validación de bandas; límite de `size` |
| `test_irsa.py` | Posición propagada; coincidencia de WISE 0855 con el YAML; épocas ordenadas y en caché; offline sin caché; el recorte se baja una sola vez; contrato de `/api/epochs` |
| `test_health.py` | `/health` responde ok |

`ruff check .` da 17 avisos, todos de línea larga (E501); ninguno funcional. El CI corre `pytest`, pero deja `ruff` en `|| true` (ver [09](09_RIESGOS_Y_AUDITORIA.md), [#80](https://github.com/mauriciovalenzuela742/SPHEREX/issues/80)).

**Lo que las pruebas no cubren:** que existan las rutas que usa el visor (`/api/grid`, `/api/frame`), que los parámetros documentados se acepten, ni `_banda()` con nombres de detector reales.

## Dependencias

Pinneadas en `requirements.txt`: `fastapi==0.115.6`, `uvicorn[standard]==0.34.0`, `httpx==0.28.1`, `pytest==8.3.4`, `ruff==0.8.6`. Las astronómicas están aparte (`requirements-astro.txt`, sin versión): `numpy`, `astropy`, `pyvo`, `astroquery`, `pillow`, `matplotlib`, `PyYAML`, `scipy`.

## Cómo correrlo

```bash
cd scaffold/backend
python -m venv .venv && source .venv/bin/activate     # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
pytest -q                                              # 46 pruebas, sin red
uvicorn app.main:app --reload --port 8000              # el visor local lo usa por proxy
```

Variables de entorno: `CORS_ORIGINS` (por defecto solo `http://localhost:5173`), `SPHEREX_CACHE` (carpeta de la caché) y `OFFLINE=1` (nunca toca internet). Estas instrucciones se infieren de `pyproject.toml`, del Dockerfile y del proxy de `vite.config.ts`; no las ejecuté en un entorno limpio con la red de IRSA.
