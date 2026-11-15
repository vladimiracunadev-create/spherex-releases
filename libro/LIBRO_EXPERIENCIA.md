# 📖 Transientes en el NASA Space Apps 2026
### Crónica de una experiencia: ciencia, alcance, trabajo resuelto y propósito pedagógico

*Libro preliminar · versión 0.2 · 15 de noviembre de 2026*

> [!IMPORTANT]
> **Cómo leer este libro.** Es una crónica personal con apoyo de documentación técnica. Cada dato lleva su origen: la página pública del equipo, los pull requests y releases del repositorio oficial, el historial de git, o mi lectura del código y de los documentos del equipo. Lo que es interpretación mía o propuesta está dicho así. No es un documento oficial del equipo ni de la NASA, y no mide el valor del trabajo de nadie. Lo marcado con **[Confirmar]** necesita que alguien del equipo lo verifique antes de compartirlo.

> [!NOTE]
> **Cambios de la versión 0.2.** Se agregan las partes de ciencia en profundidad (§4), alcance del proyecto (§5), tareas resueltas por pull request (§7) y contexto pedagógico (§8). También se corrige la versión 0.1: ahí atribuí al repositorio una división de roles que no había verificado y me describí con menos peso del que tuvo mi parte (rol **D1**, acceso a datos). Los PRs del repositorio y el documento `10_REPARTO_DE_TRABAJO.md` del equipo lo muestran.

---

## Índice

**Parte I · Contexto**
1. [El desafío](#1-el-desafío)
2. [El equipo y los roles](#2-el-equipo-y-los-roles)

**Parte II · Qué se hizo y por qué**
3. [El producto: AstroTrack](#3-el-producto-astrotrack)
4. [La ciencia en profundidad](#4-la-ciencia-en-profundidad)
5. [Alcance del proyecto](#5-alcance-del-proyecto)

**Parte III · Cómo se hizo**
6. [Cómo trabajó el equipo](#6-cómo-trabajó-el-equipo)
7. [Tareas resueltas: los pull requests](#7-tareas-resueltas-los-pull-requests)

**Parte IV · Para qué sirve enseñando**
8. [Contexto pedagógico](#8-contexto-pedagógico)

**Parte V · Mi experiencia**
9. [Mi participación](#9-mi-participación)
10. [La auditoría](#10-la-auditoría-cuando-el-contrato-y-el-código-no-coinciden)
11. [Una sola versión y el repositorio espejo](#11-una-sola-versión-y-el-repositorio-espejo)
12. [Documentación y análisis de usuarios](#12-documentación-y-análisis-de-usuarios)
13. [Herramientas e inteligencia artificial](#13-herramientas-e-inteligencia-artificial)
14. [Errores y correcciones](#14-errores-y-correcciones)
15. [Pendientes](#15-pendientes)
16. [Lecciones](#16-lecciones)
17. [Cómo contar esta experiencia](#17-cómo-contar-esta-experiencia)
18. [Apéndices](#18-apéndices)

---

# Parte I · Contexto

## 1. El desafío

El NASA Space Apps Challenge es un hackatón internacional de la NASA. En 2026 el equipo participó con el desafío **Planet X and SPHEREx**.

El enunciado, según la página pública del equipo: desde 2025 la misión SPHEREx mapea todo el cielo cada seis meses en **102 bandas de luz infrarroja cercana** y devuelve imágenes de más de mil millones de objetos. Cometas, asteroides, estrellas, enanas marrones e incluso planetas nuevos se revelan al cambiar de posición entre imágenes, pero ninguna persona puede revisar tanto dato sola. La tarea: **crear una herramienta web pública que muestre imágenes del cielo de SPHEREx y permita ver con facilidad cómo cambian en el tiempo.**

Dos ideas de ese texto guiaron todo:

- **«Pública»**: cualquiera debe poder usarla, sin cuentas ni conocimientos previos.
- **«Cómo cambian»**: el valor está en comparar fechas, no en mostrar una foto.

Evento local: **Santiago de Chile**. Comenzó el sábado 14 de noviembre de 2026 a las 10:00 (hora de Santiago) y las entregas cerraban el domingo 15 a las 11:00.

## 2. El equipo y los roles

El equipo **Transientes** se describe en su página pública como un grupo de estudiantes de ingeniería de un laboratorio de astronomía, con base en ciencia de datos. Seis integrantes figuran en la página. Cruzados con el plan del equipo (`docs/10_REPARTO_DE_TRABAJO.md`) y con los autores de los pull requests, quedan así:

| Integrante | Rol del plan | Cuenta de GitHub en los PRs | A qué se dedicó (según los PRs) |
|---|---|---|---|
| Vladimir Acuña | **D1** · acceso a datos | `vladimiracunadev-create` | SIA2, lectura por rangos, cutouts, caché, precálculo de la demo, PWA, instaladores, versión única |
| Bryan Cabezas | **D2** · fotogramas, filtros, alineación, restas | `BAFCS` | `/api/frames`, lector rápido, resta, suma por barrido, `/api/spectrum`, prototipo del explorador |
| Antonia Del Castillo | **V1** · visor del parpadeo | `antodelcast20` | Selector de longitud de onda, mejoras del explorador, vista simple, tutorial, portada |
| Juan Pablo Castillo Zepeda (dueño del equipo) | **V2** · cielo y descubrimiento | `jpcastillo714` | Visor del cielo, Gaia, SIMBAD, CatWISE, EDA, Prisma, apilado, Doppler/LVF |
| Mauricio Valenzuela Corvalan | **C** · ciencia y verificación | `mauriciovalenzuela742` | Despliegue en Vercel, Modo Misión, Spherix, Prisma inicial, pitch, revisión |
| María Javiera Valenzuela Lasen | **N** · narrativa y entrega | (sin commits propios en el historial) | Textos del Modo Misión y material de entrega, según los comentarios del código **[Confirmar]** |

> [!NOTE]
> La correspondencia entre cuenta y persona sale de las menciones en los títulos y descripciones de los PRs («el /api/frames de Bryan», «prototipo D2», «de Vladimir (#26)», «Dueña: Javiera»). **[Confirmar]** con cada persona. Los commits y PRs miden actividad en el repositorio, no el valor del aporte: la narrativa, la coordinación o el pitch no siempre pasan por git.

**Actividad en el repositorio** (commits sin merges, todas las ramas, 15-nov-2026): Juan Pablo Castillo 151; antodelcast20, 27; Bryan Cabezas, 24; Mauricio Valenzuela, 24; BAFCS, 14; Vladimir Acuña y Vladimir, 21 entre ambos. Dos cuentas pueden corresponder a la misma persona (por ejemplo «Bryan Cabezas» y «BAFCS»).

---

# Parte II · Qué se hizo y por qué

## 3. El producto: AstroTrack

Un visor web, con versiones de escritorio y Android, que compara imágenes reales de SPHEREx. Funciona sin cuentas y sin servidores propios de datos: el backend pide a IRSA lo que necesita.

| Nivel | Para quién | Qué ofrece |
|---|---|---|
| **Mirar** | Cualquiera | Portada animada, vista simple, tutorial, «Compara dos fechas» |
| **Aprender** | Estudiantes y docentes | Modo Misión, glosario, el asistente Spherix |
| **Investigar** | Aficionados y científicos | Modo ciencia: Explorador, Prisma (espectros), catálogos SIMBAD/Gaia/CatWISE, Apuntar |
| **Integrar** | Desarrolladores | URL con parámetros, API, descargas e instaladores |

**Objetos de prueba**, con datos precalculados para no depender de IRSA durante la demo: WISE 0855, Epsilon Indi B, la Estrella de Barnard, RW Aurigae y R Draconis (los 5 campos del enunciado; el YAML del equipo trae 8).

**Arquitectura en una línea:** frontend React + Vite + TypeScript (PWA) → backend FastAPI en Python → archivo IRSA, con Aladin Lite para el cielo. Detalle: [02](../documentacion/02_ARQUITECTURA.md), [03](../documentacion/03_API.md), [05](../documentacion/05_BACKEND.md), [06](../documentacion/06_FRONTEND.md); mapa de funciones: [22](../documentacion/22_MAPA_DE_FUNCIONES.md).

## 4. La ciencia en profundidad

Esta sección resume el estudio exploratorio del equipo (EDA, 14-nov-2026), hecho con imágenes reales de SPHEREx nivel 2. Las cifras vienen de scripts reproducibles del repositorio oficial (`eda/`); aquí no se midió nada de nuevo. Documentos de apoyo: [04 Datos](../documentacion/04_DATOS_SPHEREX.md), [08 Hallazgos](../documentacion/08_HALLAZGOS_CIENTIFICOS.md), [11 Glosario](../documentacion/11_GLOSARIO.md).

### 4.1 La misión y la idea de «transiente»
SPHEREx (*Spectro-Photometer for the History of the Universe, Epoch of Reionization and Ices Explorer*) es un telescopio espacial de la NASA, proyecto conjunto del JPL y Caltech. Desde mayo de 2025 mapea todo el cielo **cada ~6 meses en 102 bandas** entre 0,75 y 5 µm, con seis detectores (D1–D6). Es sensible a hidrógeno, agua, CO₂ y CO.

El nombre del equipo, *Transientes*, alude a lo que busca la herramienta: objetos que **cambian** entre observaciones, ya sea de posición (asteroides, cometas, enanas marrones con alto movimiento propio) o de brillo (estrellas variables).

### 4.2 Por qué es difícil: tres obstáculos físicos

**(a) El movimiento es de 1–2 píxeles.** El píxel mide 6,15″ y la PSF (qué tan borrosa sale una estrella puntual) 7,5–8,7″. En todo el período (1,0 a 1,4 años) los objetos de prueba se mueven: Barnard 1,8 px, WISE 0855 1,3 px, eps Ind B 1,1 px. No hay un «salto» que se vea a simple vista. Por eso la herramienta usa la **resta** (A − B) y no solo el parpadeo.

**(b) El filtro lineal variable (LVF).** SPHEREx no tiene filtros por banda: la longitud de onda cambia de forma continua a lo largo de un eje del detector. Dónde cae una fuente determina qué «color» se mide.

> **Consecuencia:** dos imágenes de la misma zona pueden no ser comparables si se tomaron a longitudes de onda distintas. Un parpadeo ingenuo mezcla ~16 anchos de banda y muestra **color, no tiempo**.

El equipo midió que igualar la λ es fácil (hay decenas de pares de épocas con la misma λ) y que la dispersión baja de **0,47 µm a 0,005–0,04 µm** al elegir «a λ igualada». Y la λ se calcula **sin bajar el archivo**: la tabla `WCS-WAVE` es fija por detector y la cabecera se pide por HTTP Range (~35 KB; ~1.900 archivos en ~2,5 minutos).

**(c) La orientación gira ~180° entre barridos.** Antes de restar hay que reproyectar a una rejilla común (TAN, norte arriba, 6,15″/px). Sin eso se compara cielo distinto.

### 4.3 Calidad de dato: la máscara `FLAGS`
Contar `FLAGS != 0` marca el **72 %** del recorte, porque el bit `SOURCE` (72,4 %) solo indica «hay una fuente conocida», no un error. Con una máscara por bits se enmascara ~4 % de los píxeles. Los bits que invalidan: `TRANSIENT`, `SUR_ERROR`, `OUTLIER` y otros; `HALO` y `BLOOM` quedan como discutibles.

### 4.4 Resultado central: ¿se mide el movimiento que predice SIMBAD?

| Campo | Medido (px) | Predicho (px) | Núcleo enmascarado |
|---|---|---|---|
| eps Ind B | −0,85 / −0,71 | −0,92 / −0,57 | 1 y 0 de 49 píxeles |
| RW Aur | +0,73 / +1,22 | 0 / 0 | 6 y 8 de 49 |
| Barnard | no se puede medir | +0,14 / +1,81 | 44 y 44 de 49 |

La alineación funciona (las estrellas de campo se desplazan una o dos décimas de píxel) y **eps Ind B se mueve lo que tiene que moverse**, dentro de ~0,15 px por eje.

### 4.5 La trampa de la saturación
Las estrellas brillantes **saturan** el sensor (Barnard, RW Aur). El núcleo viene enmascarado y queda un anillo asimétrico: en la resta aparece el **mismo par rojo/azul** que dejaría un objeto que sí se mueve. RW Aur «se mueve» 1,4 px sin moverse. Es el error científico más fácil de cometer con esta técnica, y el visor **todavía no avisa** cuando un objeto satura (riesgo R13; la tarea 7 de D2 en el plan lo pedía).

### 4.6 WISE 0855: el caso obligatorio
Una enana marrón tan fría que casi solo se ve en el infrarrojo largo. **Sí se ve en SPHEREx, pero solo con la configuración correcta:** banda D5, **10 exposiciones combinadas por visita**, primera visita (6-may-2025) contra la última (13-may-2026). La resta muestra una pareja limpia, azul al este y rojo al oeste, ~1 píxel de separación.

| Objeto | Esperado | 1 exposición por visita | 10 exposiciones por visita |
|---|---|---|---|
| eps Ind B | 0,93 px E · 0,57 px S | 1,70 px O · 3,27 px S (ruido) | 0,79 px E · 0,74 px S ✓ |
| WISE 0855 | 1,33 px O · 0,11 px N | 0,01 px O · 0,69 px S (no se ve) | 0,95 px O · 0,25 px N ✓ |

La lección de método: **con una sola exposición la «mancha roja» puede ser el cambio de longitud de onda, no movimiento.** Por eso el visor abre WISE 0855 y eps Ind B con mediana de 10 exposiciones y datos precalculados.

### 4.7 Huella espectral y movimiento propio con solo imágenes SPHEREx
Un análisis posterior del equipo (`eda/huella_espectral/`) midió el movimiento propio más paralaje de WISE 0855 usando solo imágenes de SPHEREx: **8,67″ ± 0,66″ en un año contra 8,04″ predicho**, y una huella espectral en 12 tramos logarítmicos de 0,75–5,0 µm. Una revisión científica independiente del 15-nov corrigió varias afirmaciones: los hielos de H₂O y CO₂ se ven en **absorción** (déficit), no como exceso; el CO₂ a 4,26 µm cae solo en D5; y la resolución de SPHEREx en D5 es R ≈ 120.

> [!WARNING]
> Si vas a citar estas cifras, consulta el documento original del equipo y sus correcciones. En este libro son un resumen.

### 4.8 Cómo se ve el cielo «de antes»
El visor permite comparar SPHEREx con **2MASS** y **AllWISE** (1997–2010), un «parpadeo hace 25 años contra hoy» (PR #31). Eso agrega una base temporal de décadas a la de ~1 año de SPHEREx. También se integró **CatWISE2020** (WISE 0855 no está en Gaia) y se filtraron falsos «objetos que se mueven» junto a estrellas brillantes (PR #30).

### 4.9 Qué no se comprobó (limitaciones científicas declaradas)
- **Fotometría:** no se midió brillo en ningún campo; la variabilidad de RW Aur y R Dra quedó sin medir en el estudio inicial.
- **Estrellas saturadas:** falta un método de posición (ajuste de PSF a las alas, o un detector donde no sature).
- Las colecciones `_deep` no se usaron para λ; los centroides se midieron solo en la primera y la última época.
- El espectro del visor es **indicativo, no calibrado** (la API lo dice en el campo `caveat`).
- En **R Draconis** no se pudo armar una pregunta honesta con las fotos disponibles: el centro satura y la λ cambia de 1,19 a 1,64 µm entre fotos (por eso su nivel quedó oculto en el Modo Misión).

### 4.10 Datos y rendimiento (lo que condicionó el diseño)

| Hecho medido | Consecuencia de diseño |
|---|---|
| Un recorte tarda 7,3 s de mediana (p90 10,8 s, máximo 29 s) y pesa ~3,9 MB sin importar el tamaño | Sin caché no hay demo: caché en disco y datos precalculados |
| IRSA no responde con CORS | Todo pasa por el backend y por `rewrite` de Vercel |
| Más de 4 recortes en paralelo dan 503 | Máximo 4 en paralelo y reintentos |
| El cubo DR1 por SIA2 son FITS de ~320 MB; el HiPS sí carga en Aladin | El HiPS sirve de contexto y de 102 planos; el tiempo sale de los recortes |

## 5. Alcance del proyecto

### 5.1 Qué sí cubre
1. Ver cómo cambia un punto del cielo con **imágenes reales** de SPHEREx.
2. Comparar fechas con parpadeo, resta A − B, apilado de visitas y mediana de 10 exposiciones.
3. Rodear el objeto con contexto: catálogos (SIMBAD, Gaia, CatWISE), épocas históricas (2MASS/AllWISE) y un espectro indicativo.
4. Enseñar la técnica con un recorrido guiado (Modo Misión) y un tutorial.
5. Funcionar en navegador, instalable (PWA) y como app de escritorio y Android.
6. Ser **compartible**: cada vista y cada nivel tiene su URL.
7. Funcionar con datos precalculados cuando IRSA no responde.

### 5.2 Qué decidió no hacer, y por qué

| Decisión | Motivo |
|---|---|
| No entrenar ningún modelo (R9) | El equipo no tenía GPU ni tiempo; el valor está en la visualización |
| Quitar la vista «Contaminación» (PR #27) | Decisión del equipo: no se haría; contar `FLAGS != 0` engaña (72 %) |
| Ocultar R Draconis en el Modo Misión | Sin pregunta honesta posible con esas fotos |
| Ocultar dos secciones del Prisma (huella entre pasadas y fondo del cielo) | Decisión posterior a v0.4.0; el código sigue en el repositorio **[Confirmar]** |
| Sin cuentas ni analítica | Privacidad y simplicidad |
| Sin versión para iPhone/iPad como app nativa | Exige cuenta Apple Developer de pago; queda la PWA |
| Sin FITS en el repositorio | Pesan ~70 MB; van a una caché local |

### 5.3 Límites reconocidos
Instaladores **sin firmar** y sin probar en equipos reales; backend de producción desplegado a mano; frontend sin pruebas automatizadas; contrato de la API con diferencias (ver §10); aviso de saturación pendiente; sin flujo para reportar un candidato. Hoja de ruta propuesta: [17](../documentacion/17_HOJA_DE_RUTA.md). Alcance comercial (hipótesis): [15](../documentacion/15_ALCANCE_COMERCIAL.md).

---

# Parte III · Cómo se hizo

## 6. Cómo trabajó el equipo

Según la metodología del equipo (`docs/04_METODOLOGIA.md`):

- **Roles dueños de archivos.** «Dueño» significa que esa persona decide y los demás piden, no que trabaje sola. D (datos) es el camino crítico: *«si esto no funciona, no hay proyecto»*. C (ciencia) tiene permiso para decir «esto está mal» sin proponer el arreglo. N (narrativa) empieza **desde la hora 0**, no al final.
- **Trunk-based con ramas cortas.** Nadie empuja a `main`; las ramas viven menos de 3 horas; un PR lo revisa una persona, no un agente.
- **El contrato de la API se cambia primero en el documento**, y se avisa antes de tocar el código.
- **Un *worktree* por persona** para que los agentes no se pisen.
- **Nada de FITS en el repositorio.**
- **Regla de evidencia: «ninguna cifra sin fuente» y «qué NO comprobé».** Casi todos los PRs terminan con dos secciones: *Cómo lo comprobé* y *Qué NO comprobé*. Es la costumbre más valiosa del proyecto y la he tomado como modelo.
- **13 riesgos registrados** (R1–R13) con cómo se aborda cada uno: [09](../documentacion/09_RIESGOS_Y_AUDITORIA.md).
- **Asistentes de IA con instrucciones del proyecto** (`AGENTS.md`, `CLAUDE.md`), subagentes y comandos como `/standup` o `/traspaso`.

## 7. Tareas resueltas: los pull requests

El repositorio oficial registra **76 pull requests**: **73 mezclados**, 2 abiertos (#71 y #82, míos) y 1 cerrado por ser reemplazado (#42 → #43). La tabla agrupa los mezclados por tema. Los números llevan al PR en GitHub.

> [!NOTE]
> Agrupar es mío. La columna «Quién» sale de la cuenta autora del PR. «Tareas #13, #14, #16, #18» que aparecen en algunos títulos son tareas del plan del equipo, no números de PR.

### 7.1 Datos y backend (D1 y D2)

| PR | Qué resolvió | Quién |
|---|---|---|
| [#12](https://github.com/mauriciovalenzuela742/SPHEREX/pull/12) | **Acceso a datos**: SIA2 con DR1 y QR3 en paralelo, cabeceras por HTTP Range (~35 KB), cutouts con reintentos y máximo 4 en paralelo, caché atómica, `OFFLINE=1`, `/api/epochs` (+447 líneas, 13 archivos, 12 pruebas) | Vladimir |
| [#26](https://github.com/mauriciovalenzuela742/SPHEREX/pull/26) | **Precálculo de los 5 campos de la demo** para servirlos sin IRSA en vivo (35 archivos) | Vladimir |
| [#9](https://github.com/mauriciovalenzuela742/SPHEREX/pull/9), [#13](https://github.com/mauriciovalenzuela742/SPHEREX/pull/13) | Prototipo D2: parpadeo por fechas, resta; `/api/frames` con lector por rangos y respuesta parcial | Bryan |
| [#21](https://github.com/mauriciovalenzuela742/SPHEREX/pull/21) | Suma por barrido (1/VARIANCE), `/api/spectrum`, límites de tamaño | Bryan |
| [#15](https://github.com/mauriciovalenzuela742/SPHEREX/pull/15) | Integra `/api/frames` y despliega el backend oficial | Mauricio |
| [#67](https://github.com/mauriciovalenzuela742/SPHEREX/pull/67) | Análisis Doppler/LVF y movimiento propio en detección y espectro | Juan Pablo |

### 7.2 Visor del cielo y explorador (V1 y V2)

| PR | Qué resolvió | Quién |
|---|---|---|
| [#6](https://github.com/mauriciovalenzuela742/SPHEREX/pull/6) | Visor del cielo: deslizador de λ, objeto en su posición de 2026, Gaia y SIMBAD | Juan Pablo |
| [#11](https://github.com/mauriciovalenzuela742/SPHEREX/pull/11), [#30](https://github.com/mauriciovalenzuela742/SPHEREX/pull/30) | Capa CatWISE2020 (marca WISE 0855) y quitar falsos «objetos que se mueven» junto a estrellas brillantes | Juan Pablo |
| [#16](https://github.com/mauriciovalenzuela742/SPHEREX/pull/16), [#17](https://github.com/mauriciovalenzuela742/SPHEREX/pull/17), [#20](https://github.com/mauriciovalenzuela742/SPHEREX/pull/20) | Explorador: línea de tiempo, mira, escala de 30″, 6 bandas, zoom y arrastre | Bryan, Mauricio, Antonia |
| [#31](https://github.com/mauriciovalenzuela742/SPHEREX/pull/31), [#39](https://github.com/mauriciovalenzuela742/SPHEREX/pull/39) | Parpadeo «hace 25 años contra hoy» con 2MASS y AllWISE; 2MASS en banda K | Juan Pablo |
| [#32](https://github.com/mauriciovalenzuela742/SPHEREX/pull/32), [#50](https://github.com/mauriciovalenzuela742/SPHEREX/pull/50), [#60](https://github.com/mauriciovalenzuela742/SPHEREX/pull/60) | Selector de λ: acordeón, respuesta normalizada, «Recorrer» en bucle | Juan Pablo, Antonia |
| [#45](https://github.com/mauriciovalenzuela742/SPHEREX/pull/45) – [#48](https://github.com/mauriciovalenzuela742/SPHEREX/pull/48) | Estado visible de catálogos con «Reintentar», candidatos CatWISE, WISE 0855 y eps Ind B listos, aros de época y flecha de trayectoria, apuntar con sexagesimal y «¿Qué hay aquí?» | Juan Pablo |
| [#51](https://github.com/mauriciovalenzuela742/SPHEREX/pull/51), [#54](https://github.com/mauriciovalenzuela742/SPHEREX/pull/54) | «Resta A − B» entre dos fechas; «Apilar visitas» quieto y siguiendo el movimiento | Antonia, Juan Pablo |
| [#41](https://github.com/mauriciovalenzuela742/SPHEREX/pull/41) | Enlace «Ver en WiseView» | Antonia |
| [#64](https://github.com/mauriciovalenzuela742/SPHEREX/pull/64), [#65](https://github.com/mauriciovalenzuela742/SPHEREX/pull/65) | Limpieza de estrellas, huella entre pasadas, fondo del cielo; **vista simple** y propuesta de tres niveles | Juan Pablo, Antonia |

### 7.3 Espectros (Prisma)

| PR | Qué resolvió | Quién |
|---|---|---|
| [#19](https://github.com/mauriciovalenzuela742/SPHEREX/pull/19) | Visualizador multibanda inicial (ojo infrarrojo, huella espectral) | Mauricio |
| [#34](https://github.com/mauriciovalenzuela742/SPHEREX/pull/34) | Prisma sin sonido; datos reales en el Modo Misión | Mauricio |
| [#53](https://github.com/mauriciovalenzuela742/SPHEREX/pull/53), [#55](https://github.com/mauriciovalenzuela742/SPHEREX/pull/55), [#56](https://github.com/mauriciovalenzuela742/SPHEREX/pull/56) | Espectro por pasada, «¿qué cambió?», tipo probable del astro y moléculas | Juan Pablo, Antonia |

### 7.4 Educación y comunicación (C y N)

| PR | Qué resolvió | Quién |
|---|---|---|
| [#14](https://github.com/mauriciovalenzuela742/SPHEREX/pull/14) | **Modo Misión** con fotos reales, ODS 4.4 y 4.7 | Mauricio |
| [#8](https://github.com/mauriciovalenzuela742/SPHEREX/pull/8) | **Spherix**: personalidad, motor del asistente y chat | Mauricio |
| [#37](https://github.com/mauriciovalenzuela742/SPHEREX/pull/37), [#66](https://github.com/mauriciovalenzuela742/SPHEREX/pull/66) | Portada nueva y tutorial guiado por Spherix; el tutorial empieza y termina en Epsilon Indi B | Mauricio, Antonia |
| [#57](https://github.com/mauriciovalenzuela742/SPHEREX/pull/57), [#61](https://github.com/mauriciovalenzuela742/SPHEREX/pull/61) | «¿Qué es SPHEREx?», «Cómo leer el Explorador», ayuda del mapa | Antonia |
| [#33](https://github.com/mauriciovalenzuela742/SPHEREX/pull/33) | Documento sencillo para Javiera: tareas, avances, fuentes a citar y uso de IA | Juan Pablo |
| [#52](https://github.com/mauriciovalenzuela742/SPHEREX/pull/52), [#59](https://github.com/mauriciovalenzuela742/SPHEREX/pull/59), [#68](https://github.com/mauriciovalenzuela742/SPHEREX/pull/68) | Guion del pitch de 2 minutos, pitch v2 con Backyard Worlds, pitch alternativo | Mauricio, Juan Pablo |
| [#62](https://github.com/mauriciovalenzuela742/SPHEREX/pull/62), [#63](https://github.com/mauriciovalenzuela742/SPHEREX/pull/63), [#83](https://github.com/mauriciovalenzuela742/SPHEREX/pull/83) | Coma decimal, errores traducidos al inglés, nombre AstroTrack | Antonia |
| [#85](https://github.com/mauriciovalenzuela742/SPHEREX/pull/85), [#87](https://github.com/mauriciovalenzuela742/SPHEREX/pull/87), [#88](https://github.com/mauriciovalenzuela742/SPHEREX/pull/88) | Portada nueva: logo, cielo animado, SPHEREx en órbita, opciones a la vista | Antonia |

### 7.5 Infraestructura, empaquetado y entrega

| PR | Qué resolvió | Quién |
|---|---|---|
| [#3](https://github.com/mauriciovalenzuela742/SPHEREX/pull/3), [#5](https://github.com/mauriciovalenzuela742/SPHEREX/pull/5), [#38](https://github.com/mauriciovalenzuela742/SPHEREX/pull/38), [#40](https://github.com/mauriciovalenzuela742/SPHEREX/pull/40) | Despliegue en Vercel (plan gratuito), solo `main` se despliega, despliegue de emergencia desde otra cuenta, `vercel.json` restaurado | Mauricio |
| [#43](https://github.com/mauriciovalenzuela742/SPHEREX/pull/43) | **Interfaz móvil y PWA instalable**, workflow de release por etiqueta (reemplaza a #42) | Vladimir |
| [#44](https://github.com/mauriciovalenzuela742/SPHEREX/pull/44) | **Binarios nativos**: `.exe`/`.msi`, `.dmg`, `.AppImage`/`.deb` con Tauri 2 y APK con Capacitor 6; los 5 trabajos del workflow pasan y producen 6 archivos | Vladimir |
| [#84](https://github.com/mauriciovalenzuela742/SPHEREX/pull/84) | Vista «Descargas» con detección de dispositivo e indicaciones | Vladimir |
| [#86](https://github.com/mauriciovalenzuela742/SPHEREX/pull/86) | **Versión única** desde la etiqueta; instaladores renombrados a AstroTrack | Vladimir |

### 7.6 Documentación y ciencia del equipo

| PR | Qué resolvió | Quién |
|---|---|---|
| [#1](https://github.com/mauriciovalenzuela742/SPHEREX/pull/1), [#10](https://github.com/mauriciovalenzuela742/SPHEREX/pull/10), [#24](https://github.com/mauriciovalenzuela742/SPHEREX/pull/24) | Plan principal, reparto, estado hasta el cierre (docs/11) | Mauricio, Juan Pablo |
| [#2](https://github.com/mauriciovalenzuela742/SPHEREX/pull/2), [#4](https://github.com/mauriciovalenzuela742/SPHEREX/pull/4) | **EDA con datos reales**; corrección: el cubo HiPS sí carga en Aladin | Juan Pablo |
| [#7](https://github.com/mauriciovalenzuela742/SPHEREX/pull/7) | Campos de prueba en su posición de hoy; decisión de despliegue de `/api` (ADR-001) | Mauricio |
| [#18](https://github.com/mauriciovalenzuela742/SPHEREX/pull/18), [#23](https://github.com/mauriciovalenzuela742/SPHEREX/pull/23) | Pedidos entre roles; revisión científica para C | Juan Pablo |
| [#22](https://github.com/mauriciovalenzuela742/SPHEREX/pull/22), [#58](https://github.com/mauriciovalenzuela742/SPHEREX/pull/58) | Cómo se ve el movimiento de WISE 0855; huella espectral en 5 objetivos | Juan Pablo |
| [#25](https://github.com/mauriciovalenzuela742/SPHEREX/pull/25), [#27](https://github.com/mauriciovalenzuela742/SPHEREX/pull/27), [#28](https://github.com/mauriciovalenzuela742/SPHEREX/pull/28), [#29](https://github.com/mauriciovalenzuela742/SPHEREX/pull/29), [#35](https://github.com/mauriciovalenzuela742/SPHEREX/pull/35), [#36](https://github.com/mauriciovalenzuela742/SPHEREX/pull/36), [#49](https://github.com/mauriciovalenzuela742/SPHEREX/pull/49) | Catálogos ocultos por defecto con botón; quitar «Contaminación»; llevar la demo precalculada a `main`; Gaia con campo vacío sin error; aviso de 2MASS para WISE 0855; errores legibles; la fecha de la línea de tiempo ya no cambia al pasar el mouse | Varios |

### 7.7 Lo que se ve en el conjunto
- **Mucho trabajo de calidad, no solo de funciones:** reintentos, errores que dicen la causa, avisos, «Reintentar», respeto de «reducir movimiento».
- **Un ciclo de corrección rápido:** más de un PR existe para arreglar lo del anterior (#30, #29, #36, #40).
- **Ciencia contrastada:** varios PRs cambian el texto o la técnica tras mirar los datos (#4, #34, #50).

# Parte IV · Para qué sirve enseñando

## 8. Contexto pedagógico

Esta parte responde a una pregunta: **¿qué enseña AstroTrack y cómo?** El repositorio trae decisiones educativas explícitas, sobre todo en el Modo Misión (PR #14, textos en `content/mision.ts`). Lo que sigue combina lo que el equipo dejó escrito con mi análisis, que va marcado.

### 8.1 Propósito educativo declarado
El código fija el objetivo: **ODS 4, metas 4.4 y 4.7**. La meta 4.4 trata de aumentar las competencias técnicas y profesionales; la 4.7, de la educación para el desarrollo sostenible y la ciudadanía global. En palabras del archivo: *«que un joven pase de espectador a investigador con datos reales de SPHEREx, usando el mismo visor que pide el desafío»*.

### 8.2 La historia como puerta de entrada: Tombaugh, 1930
El Modo Misión parte con tres pantallas:
1. *1930: un joven compara dos fotos.* Clyde Tombaugh, de 23 años, fotografía la misma zona dos veces, con seis días de diferencia, en el Observatorio Lowell.
2. *El truco: hacerlas parpadear.* Alterna las dos fotos con una máquina; el 18 de febrero de 1930 ve que un puntito salta: es Plutón.
3. *Ahora te toca a ti.* SPHEREx fotografía todo el cielo cada seis meses.

Es un **puente histórico**: la misma técnica de hace casi un siglo, con datos de hoy. Enlaza con el enunciado del desafío («Planet X») y con la ciencia ciudadana (el pitch menciona Backyard Worlds).

### 8.3 Los niveles y qué aprende quien los recorre
*(La columna «Aprendizaje» es mi lectura de cada nivel, no texto del equipo.)*

| Nivel | Objeto | Pregunta que se responde mirando | Aprendizaje |
|---|---|---|---|
| 1 · La estrella con prisa | Barnard | ¿Hacia dónde se movió? | Leer la resta: rojo = dónde está, azul = dónde estaba; movimiento propio |
| 2 · La enana marrón invisible | WISE 0855 | ¿Cambió la posición o el brillo? | Un objeto débil exige sumar fotos; pareja azul-rojo = posición, mancha de un color = brillo |
| 3 · Dos enanas que viajan juntas | Epsilon Indi B | ¿Hacia dónde se movieron? | Cada objeto tiene su propio camino |
| 4 · ¿Todo lo de la resta se movió? | RW Aurigae | ¿Una mancha significa que se movió? | **Desconfiar:** una mancha es un *candidato* hasta comprobarlo (saturación, estrella variable, filtro distinto) |
| (oculto) El latido de R Draconis | R Draconis | Cambio de brillo | Pareja rojo-azul = movimiento; mancha pareja = brillo |

Al final, la insignia **«Cazador de planetas»**.

> [!NOTE]
> En la exportación del código que revisé, el nivel de R Draconis está **oculto** (`oculto: true`) y su respuesta marcada como no verificada, así que el recorrido visible tiene 4 niveles. La documentación y las notas del release hablan de 5. **[Confirmar]** el estado actual.

### 8.4 Principios de diseño educativo que se ven en el código
1. **Se aprende mirando, no leyendo.** Cada pregunta se responde observando el parpadeo o la resta (decisión 2 del equipo).
2. **Del más fácil al más sutil.** Primero el salto más claro, al final el caso ambiguo (decisión 1).
3. **Un nivel que enseña a dudar.** El «nivel de la duda» (RW Aur) es la decisión pedagógica más valiosa: la habilidad científica central no es detectar, es **descartar falsos positivos**.
4. **Retroalimentación explicada, sin puntajes ni bloqueos.** Cada opción, correcta o no, trae una explicación; los errores devuelven a la imagen («Mira otra vez la resta…»).
5. **Honestidad antes que espectáculo.** Un nivel se oculta si con los datos disponibles no se puede mostrar de forma honesta (R Draconis). El campo `verificado` obliga a comprobar cada respuesta con datos reales y evidencia.
6. **Vocabulario a demanda.** Glosario al pasar el mouse o con el teclado (`[[parpadeo]]`, `[[resta]]`, `[[candidato]]`…), en lenguaje simple.
7. **Aviso cuando el color engaña.** El texto avisa si las dos fotos están a longitudes de onda distintas y un cambio de brillo puede venir del filtro.
8. **Enlace por nivel** (`?mision=1..5`): un docente puede mandar un nivel concreto a su curso.
9. **Datos reales, no ilustrativos.** El PR #34 corrigió el Modo Misión con datos reales y el #50 cambió una respuesta «ilustrativa» por una normalizada.
10. **Un puente al código.** Estaba previsto un cuaderno de Colab («¿Y si lo haces con código?»), pendiente porque el repo debía ser público; hoy el enlace está en `null`.

### 8.5 Otras piezas pedagógicas
- **Spherix**, el asistente con personalidad propia, guía el tutorial y la portada.
- **Tutorial de la vista simple** en español e inglés, que empieza y termina en Epsilon Indi B (#66), con datos precalculados para que funcione aunque IRSA falle.
- **«¿Qué es SPHEREx?» y «Cómo leer el Explorador»**, con información plegable (#57).
- **Modo ciencia como segunda capa:** quien ya entendió puede pasar a Explorador, Prisma y catálogos.
- **Glosario** del equipo y del Modo Misión.

### 8.6 Propuesta: una clase de 45 minutos
*(Propuesta mía; no existe una guía docente en el repositorio. Está sin probar con estudiantes.)*

| Minutos | Actividad | Recurso |
|---|---|---|
| 0–8 | Historia de Tombaugh y el «Planeta X» | Introducción del Modo Misión |
| 8–15 | Demostración guiada: qué es parpadeo y qué es resta | Nivel 1 (Barnard), en pantalla grande |
| 15–30 | En parejas: niveles 2 y 3, anotan «posición o brillo» y la dirección | Enlaces `?mision=2` y `?mision=3` |
| 30–40 | **Debate del nivel de la duda:** ¿qué otras causas puede tener una mancha? | Nivel 4 (RW Aur) |
| 40–45 | Cierre: qué haría falta para confirmar un candidato | Glosario, «candidato» |

**Evaluación posible:** pedir a cada estudiante que explique, en una frase, por qué una mancha de un solo color no prueba movimiento. **Lo que falta para usarlo de verdad:** una guía docente, seguimiento por estudiante (hoy no hay registro de quién completó qué) y probarlo en un aula. Análisis de usuarios: [21](../documentacion/21_TIPOS_DE_USUARIO.md), [23](../documentacion/23_RECORRIDOS_Y_OBJETIVOS.md).

### 8.7 Qué enseña el *proyecto* a quien lo estudia
Además de la astronomía, el repositorio es un caso de estudio de **método**: contrato de API antes del código, riesgos registrados desde la hora 1, «qué no comprobé» en cada PR, revisión científica independiente, y decisiones de ocultar lo que no se puede defender. Es material útil para un curso de ingeniería de software o de ciencia de datos.

---

# Parte V · Mi experiencia

## 9. Mi participación

Mi rol en el plan era **D1, acceso a datos**: según la metodología del equipo, el camino crítico. Lo que hice, con su evidencia:

| Frente | Qué hice | Evidencia |
|---|---|---|
| **Acceso a datos (D1)** | Consulta SIA2 a DR1 y QR3 en paralelo; cabeceras por HTTP Range de ~35 KB; cutouts con reintentos y máximo 4 en paralelo; caché con escritura atómica y modo `OFFLINE`; `/api/epochs`; 12 pruebas | PR #12 |
| **Demo sin IRSA** | Precálculo de los campos de prueba para servirlos sin conexión en vivo | PR #26 |
| **Móvil y PWA** | Interfaz móvil (zonas seguras, objetivos táctiles de 44 px), PWA instalable, workflow de release por etiqueta | PR #43 |
| **Instaladores** | Tauri 2 (Windows, macOS, Linux) y Capacitor 6 (Android); 5 trabajos del workflow, 6 archivos | PR #44 |
| **Descargas** | Vista «Descargas» con detección de dispositivo e indicaciones | PR #84 |
| **Versión única** | Versión desde la etiqueta de git; instaladores renombrados a AstroTrack | PR #86 |
| **Auditoría** | Revisión de deriva con el método EGM; 10 hallazgos | PR #71, issues #72–#81 |
| **Distribución pública** | Repositorio con los 7 releases y sumas SHA-256, más una página | `spherex-releases` |
| **Documentación** | 25 documentos, 16 gráficos SVG y este libro | `documentacion/`, `libro/` |

**Alcance y límites (dichos tal como están en mis PRs).** En el #12 declaré lo que no comprobé: ese día IRSA devolvió 502 sostenido en `/SIA`, así que no pude comparar los conteos de épocas ni bajar un cutout real; tampoco comprobé el criterio «los campos se sirven sin internet». En el #44 declaré que ningún instalador está firmado ni probado en un equipo real. El visor, el diseño y la ciencia de los espectros son trabajo de otras personas.

Una regla propia desde cierto punto: **trabajar solo en mi repositorio** y no tocar el oficial, porque estábamos en competencia y no estaba claro qué cambios eran bienvenidos. Los PRs #71 y #82 siguen abiertos para que el equipo decida.

## 10. La auditoría: cuando el contrato y el código no coinciden

**Por qué.** El proyecto tenía un contrato de API escrito y un código que avanzaba cada hora. Una revisión corta a mitad de camino detecta diferencias antes de la demo.

**Método.** Revisión de «deriva» con EGM, contrastando lo que el proyecto *afirma* (documentación, reglas, CI) con lo que *hace* (código, manifiestos, pruebas). Se aplicó manualmente. Complementos: `pytest` (46 pasan, sin red), `ruff` (17 avisos E501) y `tsc --noEmit` (sin errores).

| Gravedad | N.º | Ejemplos |
|---|---|---|
| Alta | 1 | `/api/resolve` y `?target=` están en el contrato pero no en el backend (#72) |
| Media | 6 | Parámetros de `/api/frames` (#73); la URL no guarda el estado (#74); endpoints fuera de contrato (#75); `_banda()` sin verificar (#76); despliegue manual (#77); caché sin gancho de arranque visible (#78) |
| Baja | 3 | `/api/health` desactualizado (#79); CI laxo (#80); CORS (#81) |

El 15-nov producción respondía `0.0.1` en `/api/health` con el código en 0.4.2: evidencia del hallazgo #77. La evidencia es de **inspección estática** y el veredicto fue *corregir antes de ampliar pruebas*, por el contrato y no por un fallo en pantalla. Detalle: [09](../documentacion/09_RIESGOS_Y_AUDITORIA.md).

## 11. Una sola versión y el repositorio espejo

**Problema.** Instaladores en 0.2.0, APK en 1.0, frontend y backend en 0.0.1, lanzamientos en 0.4.x. **Solución.** `native/estampar_version.mjs` lee la etiqueta `vX.Y.Z` y la estampa en `tauri.conf.json`, `Cargo.toml` y Android (`versionCode = mayor×10000 + menor×100 + parche`). **Resultado.** Desde v0.4.2 todo coincide; los lanzamientos anteriores conservan sus números viejos, porque cambiarlos habría sido reescribir historia.

El repositorio público `vladimiracunadev-create/spherex-releases` guarda copias exactas de los 7 releases (v0.1.0–v0.4.2), con un `SHA256SUMS` por release y una página de GitHub Pages. Las sumas prueban **integridad**, no autoría; los instaladores siguen sin firmar. Cómo verificar: [12](../documentacion/12_VERIFICAR_DESCARGAS.md); cronología: [13](../documentacion/13_CRONOLOGIA.md).

## 12. Documentación y análisis de usuarios

25 documentos agrupados: **entender** (01–06), **operar** (07, 12, 13), **evaluar** (08, 09, 18), **contextualizar** (10, 11, 14, 16), **proyectar** (15, 17, 19, 20) y **usuarios** (21–24). Criterio constante: lo que es hipótesis dice que es hipótesis. Índice: [documentacion/README.md](../documentacion/README.md).

El análisis de usuarios identifica 7 tipos, 4 objetivos y 12 casos de uso; **todo es hipótesis** (sin entrevistas ni analítica). Hallazgos útiles: dos secciones del Prisma ocultas, ningún flujo para reportar un candidato y el aviso de saturación como historia más urgente. El documento 20 responde a «¿y si otro equipo hizo lo mismo?» con el historial de git (primer commit el 14-nov a las 10:07, 337 commits) y **sin comparar con otros equipos**.

## 13. Herramientas e inteligencia artificial

| Categoría | Qué se usó |
|---|---|
| Lenguajes | Python 3.11, TypeScript, Markdown, YAML; Rust y Android vienen con Tauri y Capacitor |
| Backend | FastAPI, astropy, `pyvo`, `reproject`, HTTP Range |
| Frontend | React 18, Vite, PWA, Aladin Lite 3.8.2 |
| Nativo | Tauri 2, Capacitor 6 |
| Despliegue | Vercel, GitHub Actions, GitHub Pages |
| Datos | IRSA/IPAC (SIA2), SIMBAD, Gaia, CatWISE2020, 2MASS, AllWISE |
| Pruebas | pytest (46), ruff, `tsc --noEmit` |

**Uso de IA, dicho con claridad.** El equipo usó asistentes con instrucciones del proyecto (documentado en `AGENTS.md` y `CLAUDE.md`). Yo usé Claude Code para escribir y revisar código, ejecutar pruebas, redactar documentación y generar gráficos. Las decisiones y la verificación contra el código real fueron humanas. Detalle: [10](../documentacion/10_HERRAMIENTAS_Y_IA.md).

## 14. Errores y correcciones

| Qué pasó | Cómo se corrigió |
|---|---|
| Se contaron «24 tests» y eran 46 | Se ejecutó `pytest` |
| Afirmación errónea sobre hielos en D5/D6 | Corregida |
| Celdas de la comparativa sin respaldo | Suavizadas |
| `SECURITY.md` enlazaba a avisos privados no activados | Enlace quitado |
| Gráficos con cuadrantes invertidos y etiquetas cortadas | Rehechos tras revisarlos en pantalla |
| Anclas con emoji que GitHub no resuelve | Reemplazadas; todos los enlaces validados con un script |
| **Versión 0.1 de este libro:** roles no verificados y mi parte descrita con menos peso | Corregido en esta versión con los PRs y el reparto del equipo |

Y una **decisión de proceso**: no seguir haciendo cambios en el repositorio oficial.

## 15. Pendientes

**Del producto (del equipo):** aviso de saturación; reconciliar el contrato de la API (#72–#79); redesplegar el backend (hoy 0.0.1); pruebas de frontend y CI estricto (#80); probar y firmar instaladores; probar la demo en un móvil; flujo para reportar candidatos; cuaderno de Colab y guía docente.
**De mi repositorio** (requieren tu decisión): elegir una licencia; activar el reporte privado de vulnerabilidades; mostrar la versión visible en la web. Hoja de ruta: [17](../documentacion/17_HOJA_DE_RUTA.md).

## 16. Lecciones

1. **La ciencia: descartar es más difícil que detectar.** Una mancha en la resta es un candidato, no un hallazgo.
2. **La técnica depende del dato:** sin igualar la longitud de onda, un parpadeo muestra color y no tiempo.
3. **Un proyecto fuerte demuestra lo que afirma.** Auditar también es construir.
4. **Separar lo medido de lo supuesto** y escribir «qué NO comprobé» en cada entrega.
5. **La distribución es parte del producto:** versión coherente, sumas de verificación, instrucciones de instalación.
6. **El camino crítico es el acceso a los datos:** con recortes de 7 s y 503 ocasionales, la caché y el precálculo deciden si hay demo.
7. **Enseñar a dudar es la mejor función educativa** del proyecto.
8. **La IA acelera, pero no verifica por ti.**
9. **Trabajar dentro del alcance acordado** y preguntar antes de cambiar el repositorio de otras personas.

## 17. Cómo contar esta experiencia

- **Artículo para LinkedIn:** [articulo/LINKEDIN_BORRADOR.md](../articulo/LINKEDIN_BORRADOR.md).
- **Para el CV:** «Rol de acceso a datos en un visor de imágenes del telescopio SPHEREx (NASA Space Apps 2026): consulta SIA2/IRSA con lectura por rangos HTTP, caché y precálculo; PWA e instaladores multiplataforma con GitHub Actions; versión única desde etiquetas de git; auditoría de coherencia documentación-código; publicación verificable con SHA-256 y documentación técnica».
- **Qué no decir:** que construí el visor completo; que hay usuarios validados; que los instaladores están probados o firmados.

## 18. Apéndices

### A. Glosario mínimo
- **SPHEREx:** telescopio espacial de la NASA que mapea el cielo en infrarrojo cercano.
- **IRSA / SIA2:** archivo de datos y protocolo de consulta de imágenes.
- **LVF:** filtro lineal variable; la longitud de onda cambia a lo largo del detector.
- **Blink / resta A − B:** técnicas para ver lo que cambia entre dos fechas.
- **Movimiento propio:** desplazamiento aparente de una estrella en el cielo con los años.
- **PSF:** cómo se «derrama» la luz de una fuente puntual en la imagen.
- **Saturación:** un objeto brillante llena el sensor y puede producir un movimiento falso.
- **Candidato:** algo que parece interesante y aún no se confirmó.
- **SHA-256:** huella de un archivo para comprobar su integridad.
- **PWA:** página web instalable que funciona como app.

Más términos: [11 Glosario](../documentacion/11_GLOSARIO.md).

### B. Fuentes de la NASA y de ciencia abierta

Comprobé el 15-nov-2026 que las páginas responden (código 200), salvo el servicio SIA, que es un endpoint y no una página: ese día devolvió 502, el mismo tipo de caída intermitente que motivó la caché y el precálculo (§4.10, PR #12). Es lo que usa el proyecto, y para cada una indico para qué.

| Fuente | Qué es | Dónde se usa en este libro | Enlace |
|---|---|---|---|
| **Página de SPHEREx en IRSA** | Archivo oficial de datos de la misión (NASA/IPAC, Caltech) | §4: datos, versiones DR1/QR3, atribución | https://irsa.ipac.caltech.edu/Missions/spherex.html |
| **Tutoriales de IRSA para SPHEREx** | Cuadernos oficiales para consultar y leer los archivos | §4.2 y §9: SIA2, lectura de cabeceras, recortes | https://caltech-ipac.github.io/irsa-tutorials/spherex/ |
| **Servicio SIA2 de IRSA** | Endpoint de búsqueda de imágenes (protocolo IVOA) | §9: acceso a datos (D1) | https://irsa.ipac.caltech.edu/SIA |
| **Sitio de la misión SPHEREx** (Caltech) | Información científica de la misión | §4.1 | https://spherex.caltech.edu |
| **SPHEREx en NASA Science** | Ficha de la misión en la NASA | §4.1 | https://science.nasa.gov/mission/spherex/ |
| **CatWISE2020 en IRSA** | Catálogo de WISE con movimiento propio | §4.8 | https://irsa.ipac.caltech.edu/data/WISE/CatWISE/ |
| **NASA Space Apps Challenge** | El hackatón y su página de equipos | §1 y §2 | https://www.spaceappschallenge.org/ |
| **Backyard Worlds** | Proyecto de ciencia ciudadana que busca enanas marrones en fotos de WISE (mencionado en el pitch del equipo) | §8.2 | https://backyardworlds.org |
| **ODS 4 (Naciones Unidas)** | Educación de calidad; metas 4.4 y 4.7 citadas en el Modo Misión | §8.1 | https://sdgs.un.org/goals/goal4 |

**Otras fuentes de datos que usa el proyecto** (no son de la NASA): SIMBAD y Gaia DR3 (CDS, Estrasburgo, con espejo de Harvard), 2MASS y AllWISE (accesibles vía IRSA, 1997–2010).

**Atribución oficial que exige SPHEREx** (el visor la muestra en su pie; texto en inglés tal como lo pide la misión):

> This publication makes use of data products from the Spectro-Photometer for the History of the Universe, Epoch of Reionization and Ices Explorer (SPHEREx), which is a joint project of the Jet Propulsion Laboratory and the California Institute of Technology, and is funded by the National Aeronautics and Space Administration.

Datos vía NASA/IPAC Infrared Science Archive (IRSA).

> [!NOTE]
> Las cifras científicas de §4 (movimientos, tiempos, λ) **no salen de estas páginas**: son mediciones del estudio exploratorio del equipo sobre datos de IRSA. Las páginas de la NASA respaldan la descripción de la misión y los datos, no esos números. Los enlaces confirman que la fuente existe; no verifiqué su contenido más allá de que respondan.

### C. Fuentes de este libro
- Página del equipo: https://www.spaceappschallenge.org/2026/find-a-team/transientes/
- Repositorio oficial (solo lectura): https://github.com/mauriciovalenzuela742/SPHEREX (PRs, `docs/04_METODOLOGIA.md`, `docs/10_REPARTO_DE_TRABAJO.md`, `docs/09_EDA_HALLAZGOS.md`, `content/mision.ts`)
- Repositorio público con releases y documentación: https://github.com/vladimiracunadev-create/spherex-releases
- Sitio del desafío: https://www.spaceappschallenge.org/

### D. Mapa de la documentación
Índice completo, con rutas de lectura: [documentacion/README.md](../documentacion/README.md).

---

*Este libro es un borrador. Revísalo, corrige lo que esté mal y pide al equipo que confirme lo marcado con **[Confirmar]** antes de compartirlo.*
