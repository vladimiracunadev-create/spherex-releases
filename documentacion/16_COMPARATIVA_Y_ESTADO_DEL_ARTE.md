# ⚖️ 16 · Comparativa y estado del arte

Resumen de `docs/01_ESTADO_DEL_ARTE.md` del repositorio oficial, escrito por el equipo el 14-nov-2026 a partir de búsquedas y de la documentación de IRSA. **Las afirmaciones sobre qué no existe («ninguna herramienta pública hace hoy…») son del equipo y están limitadas por lo que buscaron.** No verifiqué por mi cuenta las características de las herramientas de terceros; si vas a citarlas, consulta sus sitios.

## 📜 El problema tiene cien años

Plutón se descubrió comparando dos placas fotográficas tomadas en fechas distintas: un punto de luz se había movido. La técnica no ha cambiado; lo que cambió es el volumen. SPHEREx mapea el cielo completo cada seis meses y entrega imágenes de más de mil millones de objetos: más de lo que una persona o un programa pueden revisar solos. Por eso el desafío apuesta por una herramienta que permita a mucha gente mirar.

## 🧰 Lo que ya existe

| Herramienta | Quién | Qué aporta | Relación con AstroTrack |
|---|---|---|---|
| **WiseView** | Voluntarios de Backyard Worlds | Parpadea coadds de WISE en el navegador; de ahí salieron cientos de descubrimientos de enanas marrones | El **modelo explícito** del desafío. No tiene eje espectral (WISE tiene 4 bandas; SPHEREx, 102) |
| **unWISE / unTimely** | Lang, Meisner, Schlafly y otros | Coadds y un catálogo de todo el cielo resuelto en el tiempo | Lección: combinar antes de mostrar es lo que deja ver fuentes débiles |
| **Aladin Lite v3** | CDS, Estrasburgo | Visor de cielo HiPS embebible, con overlays | **Es parte de AstroTrack** (versión 3.8.2); lo cita el desafío |
| **Firefly / Data Explorer de IRSA** | IPAC, Caltech | Archivo y visor para astrónomos | Hay que saber qué colección pedir y qué es un MEF; AstroTrack resuelve eso para el público general |
| **Legacy Survey viewer** | NOIRLab | Visor de cielo que sirve coadds de unWISE | Referencia |
| **ESASky** | ESA | Visor multimisión con capas temporales | Referencia de experiencia de usuario |
| **JS9** | SAO | DS9 en el navegador, manipulación de FITS en el cliente | Referencia |
| **KBMOD, HelioLinC** | Varios | Detección automática de objetos en movimiento | Otro enfoque: automático; AstroTrack es un visor para humanos |
| **ALeRCE** | Broker de alertas | Cribado de candidatos con redes pequeñas sobre recortes (alertas de ZTF) | Paso siguiente natural, planteado pero no construido |
| **Spectrophotometry Tool de IRSA** | IPAC | Fotometría calibrada | Para ciencia de verdad; AstroTrack ofrece espectros **indicativos** |

## 🎯 Dónde está el hueco

> Un visor público donde se entra una coordenada y se ve **dos ejes a la vez**: cómo cambia esa zona **en el tiempo** (el blink de toda la vida) y cómo se ve **en cada una de las 102 longitudes de onda** (el eje que WiseView no tiene y que solo SPHEREx puede dar), con el espectro de lo que uno señale generado al vuelo.

Es, a la vez, la lectura literal del enunciado y las dos viñetas más exigentes de las consideraciones del desafío (las 102 bandas y los espectros al vuelo). El estado del arte del equipo no encontró una herramienta pública que haga eso para SPHEREx.

## 🧩 Lecciones que el equipo copió de WiseView

- **Una caja de coordenadas y nada más** en la primera pantalla.
- **Los controles importan:** estiramiento, color y velocidad de parpadeo convierten un borrón en un descubrimiento.
- **Tener URL consultable:** una herramienta que se puede llamar por URL se vuelve infraestructura de otros proyectos (WiseView tiene paquetes de terceros construidos encima).

## 🔎 Cómo se compara el alcance

| Capacidad | WiseView (modelo) | Data Explorer de IRSA | AstroTrack |
|---|---|---|---|
| Parpadeo temporal para el público | Sí (WISE) | No es su propósito | Sí (SPHEREx) |
| Eje espectral (muchas bandas) | No | Archivo, no un visor guiado | Sí: deslizador de λ, Prisma y espectro indicativo |
| Pensado para quien no sabe astronomía | Lo usan aficionados y profesionales | No: es un archivo para astrónomos | Sí: vista simple, tutorial, Modo Misión |
| Resta, apilado, campo elegible | No detallado en el estado del arte | No es su propósito | Sí |
| URL consultable | Sí (tiene API) | — | Parcial |
| Datos propios | Coadds propios (unWISE) | Archivo oficial | Ninguno: consulta IRSA |
| Fotometría calibrada | No | Herramientas aparte | **No** (indicativa) |

La columna de WiseView y del Data Explorer resume lo que describe el estado del arte del equipo; no la comprobé.

## 🧪 Qué no se hizo, a propósito

Shift-and-stack, enlazado de detecciones y modelos de video quedaron **fuera de alcance**: el desafío pide un visor, y el cuello de botella real es traer y alinear los datos correctos, no reconocer el movimiento. Donde la IA aportaría aquí es en cribar candidatos, y quedó planteado como paso siguiente. → [17 Hoja de ruta](17_HOJA_DE_RUTA.md)

## 📚 Bibliografía

El repositorio oficial lleva un registro de referencias con el estado de verificación de cada una en `biblio/README.md`: **no cites desde este resumen sin mirar allá.**
