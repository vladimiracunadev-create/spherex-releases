# 🧬 20 · Originalidad y diferenciadores

**La pregunta:** *¿y si otro equipo desarrolló lo mismo?* En un hackatón donde muchos equipos eligen el mismo desafío, es probable que otros hayan entregado algo parecido. Este documento separa lo que cualquier equipo habría hecho porque el desafío lo pide, de lo que aquí es propio, y dice con qué evidencia se sostiene cada cosa y cómo responder sin exagerar.

> [!IMPORTANT]
> **No busqué qué hicieron otros equipos.** No comparé AstroTrack con ningún otro proyecto del evento, y no puedo afirmar que sea único ni que no lo sea. Todo lo que sigue habla de este proyecto: lo que dice su documentación, lo que muestra su código y lo que comprobé yo. La [plantilla del final](#-5-cómo-comparar-con-otro-equipo-plantilla) sirve para hacer esa comparación con datos cuando exista un caso concreto.

## 🔁 1. Lo que cualquier equipo habría hecho

El desafío *Planet X and SPHEREx* pide, literalmente, una herramienta web pública donde se escriban coordenadas y se vea una serie de imágenes de SPHEREx «como un videíto». Es normal que otros equipos coincidan en esto:

| Parte | Por qué es común |
|---|---|
| Caja de coordenadas + parpadeo temporal | Es la lectura literal del enunciado |
| Restar o sumar imágenes de distintas fechas | Viñeta de las consideraciones del desafío |
| Campo de visión elegible | Viñeta del desafío |
| Coordenadas en la URL | Viñeta del desafío |
| Aladin Lite y Astropy | El propio desafío los nombra |
| Overlays de Gaia o WISE, flags de contaminación | Viñetas del desafío |
| Probar con los cinco objetos del enunciado | Lo pide el texto |
| Parecerse a WiseView | Es el modelo declarado por la NASA |

**Conclusión honesta:** que otro equipo tenga estas piezas no hace que el proyecto sea «una copia»; es la consecuencia de resolver el mismo problema. Un jurado que vea varios proyectos parecidos va a juzgar por lo que va **más allá** de esta lista.

## ⭐ 2. Lo que aquí es propio, y su evidencia

Cada fila dice qué se afirma, dónde está y **qué tan comprobado está**. «Comprobado» significa que lo ejecuté o lo abrí yo; «por lectura» significa que lo leí en el código o la documentación sin operarlo.

| # | Diferenciador | Evidencia | Estado |
|---|---|---|---|
| 1 | **Dos ejes a la vez:** tiempo y espectro. Cubo HiPS con deslizador de λ, «Prisma» (color traducido y huella espectral) y espectro al vuelo | `sky/`, `prisma/` y `/api/spectrum`; [04](04_DATOS_SPHEREX.md), [06](06_FRONTEND.md) | Por lectura; el backend de espectro tiene pruebas que pasan (46 en total). Espectros **indicativos**, no calibrados |
| 2 | **Una configuración que sí muestra el movimiento de objetos débiles:** mediana de 10 exposiciones por visita en la banda D5 | Mediciones en `docs/hallazgos/`; [08](08_HALLAZGOS_CIENTIFICOS.md) | Medido por el equipo; no lo reproduje |
| 3 | **Estudio de datos reproducible:** scripts y resultados versionados (`eda/`) que miden la λ, la geometría, la latencia y la máscara de calidad | `eda/` y `eda/resultados/*.json`; [04](04_DATOS_SPHEREX.md) | Por lectura; los scripts se pueden volver a correr |
| 4 | **Movimiento medido contra lo predicho:** eps Ind B se desplaza −0,85 / −0,71 px, SIMBAD predice −0,92 / −0,57 px | `eda/05_centroides.py`; [08](08_HALLAZGOS_CIENTIFICOS.md) | Medido por el equipo; no lo reproduje |
| 5 | **Honestidad científica visible:** `caveat` obligatorio en el espectro, aviso de saturación propuesto, revisión científica independiente que corrigió afirmaciones | Contrato (`docs/03`), nota del 15-nov en `docs/hallazgos/`; [08](08_HALLAZGOS_CIENTIFICOS.md) | Por lectura. El aviso de saturación **aún no está implementado** ([17](17_HOJA_DE_RUTA.md)) |
| 6 | **Pensado para quien no sabe astronomía:** vista simple, tutorial guiado, Modo Misión de 5 niveles, glosario, español e inglés | `simple/`, `mision/`, `tutorial/`; [06](06_FRONTEND.md) | Por lectura y vista previa de la portada y la vista «Descargas»; **nunca se probó con personas nuevas** ([15](15_ALCANCE_COMERCIAL.md)) |
| 7 | **Distribución real:** web, PWA, instaladores de Windows, macOS y Linux, y APK, publicados como releases | [07](07_APPS_NATIVAS_Y_RELEASES.md) y los releases | **Comprobado:** abrí el APK, el `.exe`, el `.msi` y el `.deb` de v0.4.2 y su versión interna. Sin firmar y sin probar en equipos reales |
| 8 | **Versión única y verificable:** la etiqueta da la versión a todos los archivos, con sumas SHA-256 | PR #86, [12](12_VERIFICAR_DESCARGAS.md) | **Comprobado** en el release v0.4.2 |
| 9 | **Modo con datos precalculados:** los cinco objetos de prueba cargan sin esperar a IRSA | `data/demo/`, `public/precalc/`; [05](05_BACKEND.md) | Por lectura y 6 pruebas de `test_demo.py` que pasan |
| 10 | **Contrato, decisiones y riesgos escritos:** API documentada, ADR, 13 riesgos, auditoría de coherencia | `docs/`, [09](09_RIESGOS_Y_AUDITORIA.md) | **Comprobado** contra el código: 10 diferencias halladas, que también cuentan como evidencia de transparencia |

> [!NOTE]
> **Qué no es un diferenciador (todavía):** la «detección automática» (no existe), la fotometría de calidad científica (no la hay), la app de iPhone (no existe) ni los resultados de uso (no hay datos de usuarios).

## 💬 3. Qué decir si te preguntan

Una respuesta honesta y corta funciona mejor que una defensa larga.

| Pregunta | Respuesta sugerida | Evidencia que la respalda |
|---|---|---|
| *«Otro equipo hizo lo mismo.»* | «Es probable: el desafío pide exactamente esto, y todos partimos del mismo modelo, WiseView. Lo que sumamos son el eje espectral de 102 bandas y un estudio de datos que explica por qué la configuración por defecto no muestra el movimiento.» | Tabla de la sección 2, filas 1 a 4 |
| *«¿Qué tienen de distinto?»* | Elige **dos o tres** filas que puedas demostrar en vivo (por ejemplo la 1, la 2 y la 6) y no leas toda la lista. | La demo, y el documento [19](19_PITCH_Y_ENTREGA.md) |
| *«¿Y si les copiaron o copiaron ustedes?»* | «Nuestro historial público empieza el 14-nov a las 10:07 y se ve cómo creció el proyecto durante el evento. Las bibliotecas y datos que usamos están declarados.» | Sección 4 |
| *«¿Cuánto hizo la IA?»* | «Se declaró el uso de IA, como exige el evento. Todo lo publicado lo revisó una persona.» | [10](10_HERRAMIENTAS_Y_IA.md) |
| *«¿Es mejor que el otro?»* | «No lo sé, no vi el suyo. En lo nuestro, esto está medido y esto no.» | Esa humildad cuenta a favor ([19](19_PITCH_Y_ENTREGA.md)) |

**Lo que no hay que hacer:** decir «somos los únicos», «nadie hace esto» o «es mejor que…» sin haber comparado. El estado del arte del equipo es cuidadoso: sus afirmaciones sobre qué no existe están **limitadas por lo que buscaron**.

## 🧾 4. Autoría, atribución y trazabilidad

La mejor defensa de la originalidad es el rastro: se puede ver qué se hizo, cuándo y con qué.

**El historial de git del repositorio oficial** (corte del 2026-11-15):

| Hito | Fecha y hora (Santiago) |
|---|---|
| Primer commit: «Kit inicial de SPHEREx: documentación, skills, subagentes y esqueleto» | 14-nov 10:07 |
| Primer script de estudio de datos (`eda/01_inventario.py`) | 14-nov 11:19 |
| Primera versión de la pantalla del visor (`Visor.tsx`) | 14-nov 12:31 |
| Primer servicio de fotogramas del backend (`frames/servicio.py`) | 14-nov 15:11 |
| Total de commits | 337 (263 el 14-nov y 74 el 15-nov) |
| Nombres de autor distintos | 9 (algunas personas aparecen con más de un nombre) |

> [!WARNING]
> **Las fechas de commit son una pista, no una prueba.** Las fija quien commitea y el historial se puede reescribir. Para una disputa seria haría falta, además, el registro de la plataforma de GitHub (fechas de creación de PR y de releases) y, si procede, la organización del evento.

**Qué se declara y de dónde viene lo ajeno**

| Origen | Cómo se reconoce |
|---|---|
| Datos de SPHEREx | Atribución literal de la NASA, en el pie del visor y en [04](04_DATOS_SPHEREX.md); datos vía IRSA |
| Catálogos (SIMBAD, Gaia, CatWISE) | Citados en [04](04_DATOS_SPHEREX.md) y en el visor |
| Bibliotecas (Aladin Lite, Astropy, React, FastAPI, Tauri, Capacitor…) | Dependencias declaradas; ver [10](10_HERRAMIENTAS_Y_IA.md) |
| La idea de base (blink temporal) | Es el modelo que el propio desafío cita: WiseView |
| Código del equipo | Licencia MIT del repositorio oficial, a nombre del equipo |
| IA | Declaración obligatoria del evento; ver [10](10_HERRAMIENTAS_Y_IA.md) |
| Trabajo de este repositorio | Mío (descargas, documentación, auditoría), con asistencia de IA declarada |

**El kit previo.** El primer commit incluye un kit de documentación, skills y agentes. Las reglas del evento distinguen qué se puede preparar antes y qué no; la interpretación del equipo está en `docs/08_REGLAS_DEL_EVENTO.md` y **no la validé contra la página oficial**. Si alguien cuestiona la preparación previa, esa es la referencia, y quien decide es la organización.

## 🧪 5. Cómo comparar con otro equipo (plantilla)

Si aparece un caso concreto, compara con datos y no con impresiones. Copia la tabla y complétala **solo con lo que puedas abrir y comprobar**:

| Criterio | AstroTrack | Otro equipo |
|---|---|---|
| Enlace a la demo (¿abre desde un móvil con datos?) | <https://spherex-planet-x-tau.vercel.app/> | |
| ¿Carga los cinco objetos de prueba? ¿Cuánto tarda cada uno? | Los cinco tienen datos precalculados | |
| ¿Muestra el movimiento de WISE 0855? ¿Con qué configuración? | Sí, con D5 y 10 exposiciones por visita | |
| ¿Maneja la longitud de onda (λ) o mezcla colores y tiempo? | Agrupa por banda y λ | |
| ¿Muestra espectros o las 102 bandas? ¿Se dice que son indicativos? | Sí; `caveat` obligatorio | |
| ¿Coordenadas en la URL? | Parcial ([03](03_API.md)) | |
| ¿Funciona sin saber astronomía? | Vista simple y tutorial; sin pruebas con usuarios | |
| ¿Atribución de SPHEREx visible? | Sí | |
| ¿Declara el uso de IA? | Sí | |
| ¿Código y documentación disponibles? | Sí (repositorio oficial y esta documentación) | |
| ¿Instalable (PWA o apps)? | Sí; sin firmar | |
| ¿Qué falla o no funciona? | [09](09_RIESGOS_Y_AUDITORIA.md) lo lista | |

Una columna vacía es información: significa que no hay evidencia, no que el otro equipo sea peor.

## 🚀 6. Cómo reforzar la originalidad desde hoy

1. **Cerrar lo que está a medias**, sobre todo lo que sí lo diferencia: el aviso de saturación y la longitud de onda igualada en la API ([17 Hoja de ruta](17_HOJA_DE_RUTA.md)).
2. **Probar con personas nuevas** y dejar registro: es el supuesto más fuerte del producto y el menos medido.
3. **Publicar las mediciones** (los resultados del estudio de datos) como material citable.
4. **Conservar el rastro:** releases con sumas, historial de git y la documentación de decisiones.
5. **Hablar de lo que se hizo, no de lo que otros no hicieron.**

Ver también [16 Comparativa y estado del arte](16_COMPARATIVA_Y_ESTADO_DEL_ARTE.md), [15 Alcance comercial](15_ALCANCE_COMERCIAL.md) y [14 Preguntas y respuestas](14_PREGUNTAS_FRECUENTES.md).
