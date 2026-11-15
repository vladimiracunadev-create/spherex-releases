# ❓ 14 · Preguntas y respuestas

Las preguntas más probables de quien usa, instala, evalúa o mantiene AstroTrack, con respuestas cortas y un enlace al documento donde está el detalle. Cada respuesta dice qué está comprobado y qué no.

**Ir a:** [Uso](#-uso) · [Instalación](#-instalación-y-descargas) · [Datos y ciencia](#-datos-y-ciencia) · [Técnicas](#-preguntas-técnicas) · [Proyecto y evento](#-proyecto-y-evento) · [IA y calidad](#-ia-y-calidad)

---

## 👤 Uso

<details>
<summary><b>¿Qué puedo hacer con AstroTrack?</b></summary>

Escribes una posición del cielo (ascensión recta y declinación, en grados o sexagesimal) o eliges uno de los cinco objetos de prueba, y ves esa zona como un «videíto» con las imágenes de SPHEREx ordenadas en el tiempo. Puedes restar dos fechas (rojo = brillo que aparece, azul = brillo que estaba), apilar visitas para ver objetos débiles, elegir el campo de visión y mirar de qué está hecho un objeto en sus 102 bandas. → [01 El proyecto](01_PROYECTO.md), [06 Visor](06_FRONTEND.md)
</details>

<details>
<summary><b>¿Necesito saber astronomía?</b></summary>

No: la vista simple muestra una imagen grande con el objeto marcado y las fechas; el «Modo ciencia» abre las herramientas completas. Hay un tutorial guiado por la mascota Spherix, un Modo Misión con cinco niveles y un glosario que aparece al pasar el mouse. → [11 Glosario](11_GLOSARIO.md)
</details>

<details>
<summary><b>¿Funciona sin internet?</b></summary>

No del todo. La aplicación y los instaladores traen la interfaz, pero los datos vienen de IRSA y de los catálogos en línea. Los cinco campos de prueba tienen datos precalculados que cargan al instante, pero también se sirven por internet. Sin conexión, las apps nativas muestran una página que lo avisa. → [07 Apps y releases](07_APPS_NATIVAS_Y_RELEASES.md)
</details>

<details>
<summary><b>¿Puedo compartir lo que estoy viendo?</b></summary>

Sí, en parte. La URL guarda posición, tamaño, banda y vista (`?ra=…&dec=…&size=…&band=…`). Todavía **no** guarda el estiramiento, el modo de resta ni los cuadros por segundo, y `?target=` (buscar por nombre) no existe. → [03 API](03_API.md), [issue #74](https://github.com/mauriciovalenzuela742/SPHEREX/issues/74)
</details>

<details>
<summary><b>¿Qué significa un par rojo/azul en la resta?</b></summary>

Rojo es brillo que aparece respecto de la primera fecha y azul es brillo que estaba. Un par que se separa al avanzar el tiempo indica movimiento. **Cuidado:** en estrellas muy brillantes (Barnard, RW Aurigae) el núcleo satura y la resta muestra un par aunque no se muevan. → [08 Hallazgos](08_HALLAZGOS_CIENTIFICOS.md)
</details>

<details>
<summary><b>¿Por qué WISE 0855 casi no se ve en una imagen?</b></summary>

Es demasiado débil en una sola exposición. Se ve combinando **10 exposiciones por visita** en la banda D5 y comparando la primera visita (mayo 2025) con la última (mayo 2026): aparece una pareja azul/roja separada ~1 píxel. El visor abre así ese objeto por defecto. → [08 Hallazgos](08_HALLAZGOS_CIENTIFICOS.md)
</details>

---

## 📥 Instalación y descargas

<details>
<summary><b>¿Dónde descargo la app?</b></summary>

En la [página de descargas](https://vladimiracunadev-create.github.io/spherex-releases/) o en los [releases](https://github.com/vladimiracunadev-create/spherex-releases/releases) de este repositorio. También está en la vista «Descargas» de la web: <https://spherex-planet-x-tau.vercel.app/?vista=descargas>. → [12 Instalar y verificar](12_VERIFICAR_DESCARGAS.md)
</details>

<details>
<summary><b>¿Hay app para iPhone o iPad?</b></summary>

No nativa. Apple exige una cuenta Apple Developer de pago (99 USD al año) para publicar apps; el equipo no la tiene. La alternativa gratuita es la versión web instalable: abre <https://spherex-planet-x-tau.vercel.app> en Safari → Compartir → «Añadir a pantalla de inicio». **Nadie la ha probado en un iPhone real todavía.** → [07 Apps y releases](07_APPS_NATIVAS_Y_RELEASES.md)
</details>

<details>
<summary><b>Windows/macOS me muestra una advertencia al abrir el instalador. ¿Es seguro?</b></summary>

El aviso aparece porque los instaladores **no están firmados** (firmar exige certificados de pago). Verifica antes la suma SHA-256 contra la publicada: si coincide, el archivo es idéntico al del release. Aun así, no se probaron en equipos reales; instálalos bajo tu criterio. → [12 Instalar y verificar](12_VERIFICAR_DESCARGAS.md), [18 Seguridad](18_SEGURIDAD_Y_PRIVACIDAD.md)
</details>

<details>
<summary><b>¿Por qué los instaladores antiguos dicen 0.2.0?</b></summary>

Hasta el release v0.4.1 la versión de los instaladores de escritorio estaba escrita a mano en un archivo de configuración y no se actualizaba (y el APK decía 1.0). Desde v0.4.2 la versión sale de la etiqueta del release y coincide con ella. Los anteriores no se reconstruyeron. → [07](07_APPS_NATIVAS_Y_RELEASES.md#-una-sola-versión-desde-la-etiqueta)
</details>

<details>
<summary><b>¿Qué diferencia hay entre este repositorio y el oficial?</b></summary>

El [repositorio oficial](https://github.com/mauriciovalenzuela742/SPHEREX) es del equipo y contiene el código. Este solo guarda una **copia pública de los releases** (archivos idénticos, con sumas SHA-256), una página de descargas y esta documentación. No contiene código fuente.
</details>

<details>
<summary><b>¿Cómo sé que un archivo no fue modificado?</b></summary>

Calcula su SHA-256 y compáralo con `SHA256SUMS.txt` del mismo release. Las instrucciones por sistema están en [12](12_VERIFICAR_DESCARGAS.md). Esto prueba que el archivo es el publicado, no que el instalador sea de confianza.
</details>

---

## 🔭 Datos y ciencia

<details>
<summary><b>¿De dónde salen las imágenes?</b></summary>

De **SPHEREx**, el telescopio espacial de la NASA, vía el archivo NASA/IPAC IRSA (colecciones `spherex_dr1` y `spherex_qr3`, nivel 2). El mapa de fondo es el cubo HiPS de DR1 en 102 planos. → [04 Datos de SPHEREx](04_DATOS_SPHEREX.md)
</details>

<details>
<summary><b>¿Qué es el filtro lineal variable y por qué importa?</b></summary>

SPHEREx no usa filtros de banda: la longitud de onda cambia a lo largo del detector, así que dónde cae una estrella decide a qué «color» se la mide. Dos fotos de la misma zona pueden ser de colores distintos, y un parpadeo ingenuo mostraría color en vez de tiempo. El proyecto agrupa por banda y por longitud de onda, y en WISE 0855 igualarla bajó la dispersión de 0,474 µm a 0,035 µm. → [04](04_DATOS_SPHEREX.md)
</details>

<details>
<summary><b>¿El movimiento que se ve es real?</b></summary>

En eps Ind B sí, y está medido: el centroide se desplaza −0,85 / −0,71 px (oeste / norte) contra −0,92 / −0,57 px que predice SIMBAD (dentro de ~0,15 px). En estrellas saturadas solo vale lo cualitativo. → [08 Hallazgos](08_HALLAZGOS_CIENTIFICOS.md)
</details>

<details>
<summary><b>¿Los espectros son científicamente válidos?</b></summary>

Son **indicativos**: fotometría de apertura con la luz zodiacal restada, y la respuesta de la API incluye siempre un campo `caveat` que lo dice. Para fotometría calibrada existe la herramienta de espectrofotometría de IRSA. Quien cite estas cifras debe consultar el análisis original y sus correcciones. → [03 API](03_API.md), [08](08_HALLAZGOS_CIENTIFICOS.md)
</details>

<details>
<summary><b>¿Descubre planetas o cuerpos nuevos solo?</b></summary>

No. Es un **visor**: la persona detecta. No es un detector automático ni un modelo de video. Una capa de aprendizaje automático podría sumarse encima, pero no reemplaza al visor. El modo «descubrimiento» ayuda a mirar, y sus clasificaciones son orientativas. → [01](01_PROYECTO.md)
</details>

<details>
<summary><b>¿Por qué el visor usa la «posición propagada» y no la del enunciado?</b></summary>

La coordenada que el enunciado da para WISE 0855 es de ~2010; el objeto se mueve ~8″ por año y hoy está a unos 130″ de allí. Con un campo chico, quedaría fuera de la imagen. El visor propaga cada posición con su movimiento propio a la fecha de cada imagen. → [01](01_PROYECTO.md)
</details>

---

## 🔧 Preguntas técnicas

<details>
<summary><b>¿Por qué hay un backend si IRSA es público?</b></summary>

Porque IRSA no responde con CORS (salvo el cubo HiPS): el navegador no puede pedirle SIA2 ni archivos directo. El backend FastAPI consulta, alinea, resta y entrega PNG listos. → [02 Arquitectura](02_ARQUITECTURA.md)
</details>

<details>
<summary><b>¿Qué tan rápido es?</b></summary>

Un recorte de IRSA tarda 7,3 s de mediana (p90 10,8 s, máximo 29 s). Por eso hay caché, lector por rangos HTTP, respuestas parciales con `pending` y cinco campos precalculados que cargan al instante. Coordenadas arbitrarias dependen de IRSA en vivo y pueden tardar. → [05 Backend](05_BACKEND.md)
</details>

<details>
<summary><b>¿Cuántas pruebas tiene y pasan?</b></summary>

El backend tiene 46 pruebas, todas sin red, y pasan (ejecutadas el 2026-11-15). El frontend no tiene pruebas automatizadas; el CI solo lo compila. Las pruebas no cubren las rutas que el visor usa (`/api/grid`, `/api/frame`) ni los parámetros documentados. → [05](05_BACKEND.md), [09](09_RIESGOS_Y_AUDITORIA.md)
</details>

<details>
<summary><b>¿Qué falla o está pendiente hoy?</b></summary>

La auditoría encontró 10 diferencias entre el contrato de la API y el código (1 alta, 6 medias, 3 bajas); ninguna impide la demo. Lo más importante: `/api/resolve` está documentado y no existe. El backend en producción seguía en la versión 0.0.1 y se despliega a mano. → [09 Riesgos y auditoría](09_RIESGOS_Y_AUDITORIA.md), [17 Hoja de ruta](17_HOJA_DE_RUTA.md)
</details>

<details>
<summary><b>¿Se puede usar desde otro sitio o proyecto?</b></summary>

El diseño lo busca (URL consultable, como WiseView, que se volvió infraestructura de otros proyectos), pero hoy la URL guarda solo parte del estado y la API no está documentada como pública estable. Antes de depender de ella, ver [03 API](03_API.md).
</details>

---

## 🏁 Proyecto y evento

<details>
<summary><b>¿Qué es este proyecto y quién lo hizo?</b></summary>

AstroTrack es el resultado de un equipo de seis personas en el NASA Space Apps Challenge 2026, sede Santiago, para el desafío *Planet X and SPHEREx*. Mi participación y sus límites están descritos en el [README](../README.md#-mi-participación).
</details>

<details>
<summary><b>¿Ganó algo?</b></summary>

No lo sé y esta documentación no afirma ningún resultado. El jurado evalúa impacto, creatividad, validez, relevancia y presentación; los resultados los publica la organización del evento.
</details>

<details>
<summary><b>¿Y si otro equipo hizo lo mismo?</b></summary>

Es probable: el desafío pide exactamente esto y todos parten del mismo modelo, WiseView. Lo básico (coordenadas, parpadeo, resta, URL) lo tendrán muchos; lo que aquí va más allá es el eje espectral de 102 bandas, un estudio de datos reproducible y la distribución real con instaladores. No comparé con otros equipos, así que no afirmo que sea único. → [20 Originalidad y diferenciadores](20_ORIGINALIDAD_Y_DIFERENCIADORES.md)
</details>

<details>
<summary><b>¿Con qué licencia se publica?</b></summary>

El repositorio oficial incluye una licencia **MIT** a nombre del equipo. Este repositorio (descargas y documentación) todavía no tiene licencia propia. Los datos de SPHEREx son de la NASA y exigen la atribución literal que aparece en [04](04_DATOS_SPHEREX.md). → [15 Alcance comercial](15_ALCANCE_COMERCIAL.md)
</details>

<details>
<summary><b>¿Cómo se organizó el trabajo en 24 horas?</b></summary>

Con roles (D1, D2, V1, V2, C, N), un contrato de API fijado en la hora 1, una rama y un *worktree* por persona, y reglas como «ninguna cifra sin fuente» y «qué NO comprobé» obligatorio en cada PR. → [01](01_PROYECTO.md), [10](10_HERRAMIENTAS_Y_IA.md)
</details>

---

## 🤖 IA y calidad

<details>
<summary><b>¿Se usó IA?</b></summary>

Sí, y el evento exige declararlo. El equipo usó agentes de IA configurados en el repositorio; en mi parte usé Claude Code y el método de revisión EGM. Todo lo publicado aquí lo revisé yo. → [10 Herramientas y uso de IA](10_HERRAMIENTAS_Y_IA.md)
</details>

<details>
<summary><b>¿Spherix, la mascota, usa un modelo de lenguaje?</b></summary>

No. Su motor es **determinista**: entiende lo que escribes con reglas y responde con textos escritos por una persona. Un chat con un modelo de lenguaje fue una idea opcional del diseño, y se declararía si se hiciera. → [06 Visor](06_FRONTEND.md)
</details>

<details>
<summary><b>¿Cómo sé que esta documentación es fiable?</b></summary>

Está hecha leyendo el repositorio oficial y contrastándola con lo que ejecuté (`pytest`, `ruff`, `tsc`, los instaladores). Donde algo es inferencia mía lo dice. Es una fotografía del 2026-11-15: ante una diferencia, manda el repositorio oficial. → [README de documentación](README.md#-vigencia)
</details>

<details>
<summary><b>Encontré un error. ¿Qué hago?</b></summary>

En esta documentación, abre un [issue en este repositorio](https://github.com/vladimiracunadev-create/spherex-releases/issues). En el código de la aplicación, el [repositorio oficial](https://github.com/mauriciovalenzuela742/SPHEREX/issues). Para algo de seguridad, ver [SECURITY.md](../SECURITY.md).
</details>
