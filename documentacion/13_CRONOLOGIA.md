# 13 · Cronología de versiones

Los siete releases se publicaron en el repositorio oficial durante el hackatón, el **15 de noviembre de 2026** (horas en UTC; Santiago va 3 horas por detrás). Los de este repositorio son copias exactas hechas el mismo día. Cada versión es un corte de lo que el equipo había mezclado en `main`, no un hito planificado.

| Versión | Publicado (UTC) | Qué trae |
|---|---|---|
| **v0.1.0** | 00:48 | Solo la PWA (zip estático). El visor ya estaba publicado en Vercel |
| **v0.2.0** | 01:13 | Primeros binarios nativos: Windows, macOS, Linux y Android (PR #44) |
| **v0.3.0** | 02:47 | Estado visible de los catálogos con «Reintentar» y candidatos de CatWISE que se mueven (#45); WISE 0855 y eps Ind B abren listos con datos precalculados (#46); aros de época y flecha de trayectoria en el mapa (#47); «Resta A − B» entre dos fechas (#51); apuntar y descubrir con sexagesimal, «¿Qué hay aquí?» y zona al azar (#48); guion del pitch (#52); Prisma con espectro por pasada y tipo probable del astro (#53); «Apilar visitas» (#54) |
| **v0.3.1** | 03:24 | Hasta el PR #56: espectro SPHEREx y tipo probable en el modo descubrimiento, ajustes de Prisma |
| **v0.4.0** | 09:58 | Hasta el PR #65: vista simple y Modo ciencia, tutorial en español e inglés, comentarios del público, Visor con limpieza de estrellas, huella entre pasadas y fondo del cielo, propuesta de interfaz en tres niveles |
| **v0.4.1** | 12:51 | Tutorial que empieza y termina en Epsilon Indi B (#66); análisis Doppler/LVF y movimiento propio en detección y espectro (#67); pitch alternativo y arreglos para cuando IRSA no responde (#68) |
| **v0.4.2** | 13:44 | Nombre **AstroTrack** y errores en inglés (#83); vista «Descargas» (#84); portada nueva con logo, cielo animado y Spherix (#85); **versión única desde la etiqueta** (#86) |

Los números `#NN` son pull requests del [repositorio oficial](https://github.com/mauriciovalenzuela742/SPHEREX/pulls?q=is%3Apr+is%3Amerged).

## Qué cambió de v0.4.1 a v0.4.2 en los archivos

| | v0.4.1 y anteriores | v0.4.2 |
|---|---|---|
| Nombre de los archivos | `AstroTRACK-…` | `AstroTrack-…` |
| Versión en los instaladores de escritorio | 0.2.0 | 0.4.2 |
| `versionName` del APK | 1.0 | 0.4.2 (`versionCode` 402) |
| Versión del frontend y del backend | 0.0.1 | 0.4.2 |

## Cómo se ve en la línea de tiempo del evento

El hackatón empezó el sábado 14-nov-2026 a las 10:00 (hora de Santiago) y las entregas cerraban el domingo 15-nov a las 11:00. Los siete releases salieron entre las 21:48 del sábado y las 10:44 del domingo, hora de Santiago.

Fechas y horas tomadas de los releases del repositorio oficial. Los releases publicados aquí llevan la fecha de la copia, no la original.
