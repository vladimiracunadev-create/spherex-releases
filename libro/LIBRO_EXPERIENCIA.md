# 📖 Transientes en el NASA Space Apps 2026
### Crónica de una experiencia: ciencia, proceso de trabajo y lo que quedó escrito

*Libro preliminar · versión 0.1 · 15 de noviembre de 2026*

> [!IMPORTANT]
> **Cómo leer este libro.** Es una crónica personal con apoyo de documentación técnica. Cada dato lleva su origen: lo que sale de la página pública del equipo, de los releases, del historial de git o de mi lectura del código. Lo que es interpretación mía o hipótesis está dicho así. No es un documento oficial del equipo ni de la NASA, y no mide el valor del trabajo de nadie. Lo marcado con **[Confirmar]** necesita que alguien del equipo lo verifique antes de publicarse.

---

## Índice

1. [El desafío](#1-el-desafío)
2. [El equipo](#2-el-equipo)
3. [La ciencia en dos páginas](#3-la-ciencia-en-dos-páginas)
4. [El producto: AstroTrack](#4-el-producto-astrotrack)
5. [Cómo trabajó el equipo](#5-cómo-trabajó-el-equipo)
6. [Veintiséis horas: la línea de tiempo](#6-veintiséis-horas-la-línea-de-tiempo)
7. [Mi participación](#7-mi-participación)
8. [La auditoría: cuando el contrato y el código no coinciden](#8-la-auditoría-cuando-el-contrato-y-el-código-no-coinciden)
9. [Una sola versión para todo](#9-una-sola-versión-para-todo)
10. [Distribución: el repositorio espejo](#10-distribución-el-repositorio-espejo)
11. [El estudio del repositorio y la documentación](#11-el-estudio-del-repositorio-y-la-documentación)
12. [Quién usaría esto: análisis de usuarios](#12-quién-usaría-esto-análisis-de-usuarios)
13. [Herramientas, lenguajes, datos e inteligencia artificial](#13-herramientas-lenguajes-datos-e-inteligencia-artificial)
14. [Errores y correcciones en el camino](#14-errores-y-correcciones-en-el-camino)
15. [Lo que quedó pendiente](#15-lo-que-quedó-pendiente)
16. [Lecciones](#16-lecciones)
17. [Cómo contar esta experiencia](#17-cómo-contar-esta-experiencia)
18. [Apéndices](#18-apéndices)

---

## 1. El desafío

El NASA Space Apps Challenge es un hackatón internacional de la NASA. En 2026, el equipo participó con el desafío **Planet X and SPHEREx**.

El enunciado, según la página pública del equipo: desde 2025, la misión SPHEREx de la NASA mapea todo el cielo cada seis meses en **102 bandas de luz infrarroja cercana** y devuelve imágenes de más de mil millones de objetos. Cometas, asteroides, estrellas, enanas marrones e incluso planetas nuevos se revelan al cambiar de posición entre imágenes, pero ninguna persona puede revisar tanto dato sola. La tarea: **crear una herramienta web pública que muestre imágenes del cielo de SPHEREx y permita ver con facilidad cómo cambian en el tiempo.**

Dos ideas de ese texto guiaron todo lo demás:

- **«Pública»**: cualquiera debe poder usarla, sin cuentas ni conocimientos previos.
- **«Cómo cambian»**: el valor no está en mostrar una foto, sino en comparar fechas.

El evento local fue en **Santiago de Chile**. Empezó el sábado 14 de noviembre de 2026 a las 10:00 (hora de Santiago) y las entregas cerraban el domingo 15 a las 11:00.

## 2. El equipo

El equipo se llama **Transientes**. Según su página pública se describe como un grupo de estudiantes de ingeniería de un laboratorio de astronomía, con base en ciencia de datos. Los seis integrantes que figuran:

| Integrante | Rol en la plataforma |
|---|---|
| Juan Pablo Castillo Zepeda | Dueño del equipo |
| Bryan Cabezas | Integrante |
| Antonia Del Castillo | Integrante |
| Mauricio Osvaldo Valenzuela Corvalan | Integrante |
| Vladimir Bernardo Acuña Valdebenito | Integrante (autor de este libro) |
| María Javiera Valenzuela Lasen | Integrante |

**Actividad en el repositorio** (commits sin merges, todas las ramas, conteo del 15-nov-2026): Juan Pablo Castillo 151; antodelcast20, 27; Bryan Cabezas, 24; Mauricio Valenzuela, 24; BAFCS, 14; Vladimir Acuña y Vladimir, 21 entre ambos.

> [!NOTE]
> Los commits miden actividad en el repositorio, no el valor del aporte. «antodelcast20» se asocia con Antonia por el nombre de usuario, y «BAFCS» no se pudo asociar con nadie. **[Confirmar]** Tampoco hay commits de María Javiera, lo que no significa que no aportara: la presentación, el pitch o la coordinación no pasan por git.

El núcleo del visor, el backend y la mayor parte de las funciones salen del trabajo de Juan Pablo Castillo, según el historial. Esa es la razón por la que este libro cuenta el proyecto *desde fuera del núcleo*: lo que sigue es lo que hizo falta alrededor.

## 3. La ciencia en dos páginas

### Qué es SPHEREx
Un telescopio espacial de la NASA que fotografía todo el cielo en el infrarrojo cercano, de 0,75 a 5 micrómetros, con 6 detectores (D1 a D6) y 102 bandas. Los datos se publican en el archivo **IRSA**, del Caltech/IPAC, y se consultan con el protocolo **SIA2**.

### Por qué el movimiento es difícil de ver
Un objeto cercano (un asteroide, una enana marrón, un hipotético Planeta X) cambia de lugar entre dos visitas. Pero en SPHEREx ese cambio es pequeño: a veces de **uno o dos píxeles**. Si la imagen es una sola exposición, el ruido lo tapa.

### El filtro lineal variable
Cada detector usa un filtro cuya longitud de onda cambia a lo largo de la imagen. Eso significa que **dos fotos del mismo punto del cielo, a la misma banda, no son idénticas en color**: hay que agruparlas por banda y por longitud de onda para compararlas bien. Es la razón por la que el visor trabaja con una `wavelength_um` por fotograma. Ver [04 Datos SPHEREx](../documentacion/04_DATOS_SPHEREX.md).

### La técnica del visor
1. **Parpadeo** (*blink*): alternar dos fechas como hacía Clyde Tombaugh con las placas de Plutón en 1930.
2. **Resta A − B**: lo que no se movió se cancela; lo que se movió queda como un par rojo y azul.
3. **Apilado y mediana de 10 exposiciones**: para sacar a la luz un movimiento débil.
4. **Espectro por pasada**, indicativo, no calibrado, con una advertencia (`caveat`) en la API.

### La trampa de la saturación
Una estrella muy brillante puede saturar el sensor y producir un «movimiento falso». El equipo lo registró como riesgo (R13), y el nivel 4 del Modo Misión lo enseña con el caso RW Aur. Hoy el visor **todavía no avisa** cuando un objeto satura, y esa es la historia más urgente del análisis de usuarios.

Detalle científico y hallazgos: [08 Hallazgos científicos](../documentacion/08_HALLAZGOS_CIENTIFICOS.md), [11 Glosario](../documentacion/11_GLOSARIO.md).

## 4. El producto: AstroTrack

Un visor web, con versiones de escritorio y Android, que compara imágenes reales de SPHEREx. Funciona sin cuentas y sin servidores propios para los datos: el backend pide a IRSA lo que necesita.

**Tres niveles de uso** (propuesta de interfaz del equipo):

| Nivel | Para quién | Qué ofrece |
|---|---|---|
| Mirar | Cualquiera | Portada animada, vista simple, tutorial, «Compara dos fechas» |
| Aprender | Estudiantes y docentes | Modo Misión de 5 niveles, glosario, el asistente Spherix |
| Investigar | Aficionados y científicos | Modo ciencia: Explorador, Prisma (espectros), catálogos SIMBAD/Gaia/CatWISE, Apuntar |
| Integrar | Desarrolladores | URL con parámetros, API, descargas e instaladores |

**Objetos de prueba** con datos precalculados: Epsilon Indi B, WISE 0855, RW Aur y la estrella de Barnard, entre otros. Sirven para que la demo no dependa de que IRSA responda.

**Arquitectura en una línea:** frontend React + Vite + TypeScript (PWA) → backend FastAPI en Python → archivo IRSA, con Aladin Lite para el cielo. Detalle: [02 Arquitectura](../documentacion/02_ARQUITECTURA.md), [05 Backend](../documentacion/05_BACKEND.md), [06 Frontend](../documentacion/06_FRONTEND.md), [03 API](../documentacion/03_API.md).

Mapa completo de funciones y estado: [22 Mapa de funciones](../documentacion/22_MAPA_DE_FUNCIONES.md).

## 5. Cómo trabajó el equipo

Lo que sale de las reglas escritas del proyecto (`AGENTS.md`, `CONTRIBUTING.md`):

- **Nadie commitea a `main`.** Todo entra por pull request.
- **Un *worktree* por persona**, para que dos agentes no se pisen en la misma carpeta. El riesgo R7 («dos agentes se pisan y rompen `main`») lo reconoce explícitamente.
- **Ninguna cifra sin fuente.** Evita afirmar algo falso delante del jurado (R8).
- **Nada de archivos FITS en el repositorio**, por tamaño.
- **Roles por frente** (D1, D2, V1, V2, C, N) para evitar la dispersión (R6). **[Confirmar]** quién tenía cada rol.
- **Publicar fuera del repositorio requiere aprobación.**
- **Asistentes de IA con instrucciones del proyecto:** subagentes para explorar datos y revisar la ciencia y la interfaz, y comandos como `/standup`, `/traspaso` o `/chequeo-demo`.
- **Un registro de 13 riesgos** (R1–R13), con cómo se aborda cada uno: [09 Riesgos y auditoría](../documentacion/09_RIESGOS_Y_AUDITORIA.md).

Esto es notable en un hackatón: la disciplina de proceso (PRs, riesgos, reglas de evidencia) es poco común cuando el reloj aprieta.

## 6. Veintiséis horas: la línea de tiempo

Siete releases entre el sábado 14, 21:48, y el domingo 15, 10:44 (hora de Santiago). Los números `#NN` son pull requests del repositorio oficial.

| Versión | Qué trajo |
|---|---|
| **v0.1.0** | La PWA en un zip estático; el visor ya estaba en Vercel |
| **v0.2.0** | Primeros binarios nativos: Windows, macOS, Linux y Android |
| **v0.3.0** | Estado de catálogos con «Reintentar»; candidatos CatWISE; resta A − B; apuntar con sexagesimal; Prisma con espectro por pasada; «Apilar visitas»; guion del pitch |
| **v0.3.1** | Espectro y tipo probable en modo descubrimiento |
| **v0.4.0** | Vista simple y Modo ciencia, tutorial en español e inglés, comentarios, propuesta de tres niveles |
| **v0.4.1** | Tutorial con Epsilon Indi B; análisis Doppler/LVF; arreglos cuando IRSA no responde |
| **v0.4.2** | Nombre **AstroTrack**, vista «Descargas», portada nueva y **versión única desde la etiqueta** |

El ritmo es el de un equipo que corta una versión cada pocas horas: cada release es un corte de lo mezclado en `main`, no un hito planificado. Detalle en [13 Cronología](../documentacion/13_CRONOLOGIA.md).

## 7. Mi participación

Mi aporte no fue el núcleo del visor, sino lo que lo rodea: **verificar, versionar, distribuir y documentar**.

| Frente | Qué hice | Evidencia |
|---|---|---|
| Auditoría | Revisión de deriva con el método EGM; 10 hallazgos; informe preliminar | PR #71, issues #72–#81 |
| Versión | Versión única tomada de la etiqueta de git en instaladores y APK | PR #86, release v0.4.2 |
| Descargas | Vista «Descargas» en la web, con iconos e indicaciones | PR #84 |
| Distribución | Repositorio público con los 7 releases y sumas SHA-256, más una página | `spherex-releases` |
| Documentación | 25 documentos, 16 gráficos SVG, preguntas frecuentes, alcance comercial, comparativa, hoja de ruta, seguridad, pitch, originalidad, usuarios y casos de uso | `documentacion/` |
| Comunicación | Textos para informar al equipo y borrador para LinkedIn | este repo |

Con una regla propia desde un punto del camino: **trabajar solo en mi repositorio** y no tocar el oficial del equipo, porque estábamos en una competencia y no estaba claro qué cambios eran bienvenidos. Eso explica que el informe de la auditoría haya quedado como PR e issues (que el equipo decide aceptar) y que, después, todo lo demás viva en el repo público.

> [!NOTE]
> **[Confirmar]** Los PRs #84 y #86 se mezclaron en el repositorio oficial antes de esa regla. Desde entonces no hubo más cambios allí.

## 8. La auditoría: cuando el contrato y el código no coinciden

### Por qué auditar
El proyecto tenía un contrato de API escrito (`docs/03`) y un código que avanzaba cada hora. En ese ritmo, los documentos se quedan atrás. Una revisión corta a mitad de camino detecta diferencias antes de la demo.

### Método
Una revisión de «deriva» basada en el método EGM: contrastar lo que el proyecto *afirma* (documentación, reglas, CI) con lo que *hace* (código, manifiestos, pruebas). Se aplicó **manualmente**, sin agentes automáticos. Complementos ejecutados: `pytest` (46 pruebas, todas pasan, sin red), `ruff` (17 avisos de longitud de línea) y `tsc --noEmit` (sin errores).

### Qué se encontró

| Gravedad | Cantidad | Ejemplos |
|---|---|---|
| Alta | 1 | `/api/resolve` y `?target=` están en el contrato pero no en el backend (#72) |
| Media | 6 | Parámetros de `/api/frames` no aceptados (#73); la URL no guarda el estado (#74); endpoints fuera de contrato (#75); `_banda()` sin verificar (#76); despliegue manual (#77); caché sin gancho de arranque visible (#78) |
| Baja | 3 | `/api/health` desactualizado (#79); CI laxo (#80); CORS (#81) |

**Un dato de la propia auditoría:** el 15-nov, producción respondía `0.0.1` en `/api/health` mientras el código estaba en 0.4.2, es decir, el despliegue manual no se había repetido. Es la evidencia concreta del hallazgo #77.

### Evidencia y límites
La evidencia es de **inspección estática**: los hallazgos no se reprodujeron contra la API viva. El veredicto fue *corregir antes de ampliar pruebas*, por el contrato y no por un fallo en pantalla. Nada de esto impide la demo.

Informe y tabla completa: [09 Riesgos y auditoría](../documentacion/09_RIESGOS_Y_AUDITORIA.md).

## 9. Una sola versión para todo

**El problema.** Los instaladores de escritorio decían 0.2.0 por dentro, el APK decía 1.0, y el frontend y el backend 0.0.1, mientras los lanzamientos iban por 0.4.x. Para una persona del jurado, eso transmite descuido.

**La solución.** Un script (`native/estampar_version.mjs`) que lee la **etiqueta de git** (`vX.Y.Z`) y la estampa en `tauri.conf.json`, `Cargo.toml` y el Android (`versionName` y `versionCode = mayor*10000 + menor*100 + parche`). La acción de release lo ejecuta al publicar. Producto y archivos pasan a llamarse **AstroTrack**.

**El resultado.** Desde la v0.4.2 todo coincide: instaladores, APK y web. Los lanzamientos anteriores conservan sus números viejos por dentro, porque cambiarlos habría sido reescribir historia.

Cuadro comparativo: [13 Cronología](../documentacion/13_CRONOLOGIA.md) y [07 Apps nativas y releases](../documentacion/07_APPS_NATIVAS_Y_RELEASES.md).

## 10. Distribución: el repositorio espejo

Los releases oficiales viven en el repositorio del equipo. Para tener además una copia propia, estable y **verificable** de las descargas, se creó el repositorio público `vladimiracunadev-create/spherex-releases`:

- Los **7 releases** (v0.1.0 a v0.4.2), copias exactas de los originales.
- Un archivo `SHA256SUMS` por release, para verificar que el archivo es el publicado.
- Una **página en GitHub Pages** que enlaza a cada descarga y consulta el último release.
- Una vista **«Descargas»** dentro del propio visor, que apunta al repositorio oficial y, si no existe el archivo, al espejo.

Cómo verificar una descarga: [12 Verificar descargas](../documentacion/12_VERIFICAR_DESCARGAS.md).

**Límite honesto:** los instaladores están **sin firmar** y **sin probar en equipos reales**; las sumas SHA-256 prueban integridad, no autoría.

## 11. El estudio del repositorio y la documentación

Una vez fijada la regla de trabajo solo en el repositorio propio, el estudio del oficial fue de **solo lectura** (una exportación local del estado `5b599b9`). De ahí salió la documentación: 25 documentos agrupados así.

| Bloque | Documentos |
|---|---|
| Entender | 01 Proyecto · 02 Arquitectura · 03 API · 04 Datos SPHEREx · 05 Backend · 06 Frontend |
| Operar | 07 Apps nativas y releases · 12 Verificar descargas · 13 Cronología |
| Evaluar | 08 Hallazgos científicos · 09 Riesgos y auditoría · 18 Seguridad y privacidad |
| Contextualizar | 10 Herramientas e IA · 11 Glosario · 14 Preguntas frecuentes · 16 Comparativa |
| Proyectar | 15 Alcance comercial · 17 Hoja de ruta · 19 Pitch y entrega · 20 Originalidad |
| Usuarios | 21 Tipos · 22 Mapa de funciones · 23 Recorridos y objetivos · 24 Casos de uso |

Criterio que se mantuvo: **lo que es hipótesis dice que es hipótesis** (por ejemplo, el análisis comercial está etiquetado con [Hipótesis]). Y un principio de evidencia: se distingue lo medido, lo leído en el código y lo supuesto.

Índice: [documentacion/README.md](../documentacion/README.md).

### Sobre la originalidad
El documento 20 responde a una pregunta incómoda: *¿qué pasa si otro equipo hizo lo mismo?* Con el historial de git como evidencia (primer commit el 14-nov-2026 a las 10:07 hora de Santiago, 337 commits) se muestra qué es propio y qué depende de otras herramientas. **No se comparó con trabajos de otros equipos**, porque no hay acceso a ellos.

## 12. Quién usaría esto: análisis de usuarios

Siete tipos de usuario, con objetivo, ruta, frustraciones y cobertura actual (🟢 🟡 🔴):

| # | Usuario | Busca |
|---|---|---|
| U1 | Curiosa o curioso | «¿Qué es esto?», en 2 minutos y desde el móvil |
| U2 | Docente o estudiante | Una clase: «cómo se descubrió Plutón» |
| U3 | Ciencia ciudadana | Participar y contribuir |
| U4 | Aficionado | Ver si algo se mueve |
| U5 | Investigación | Candidatos con herramientas confiables |
| U6 | Jurado | Entender y evaluar rápido |
| U7 | Integrador | Abrir el visor desde su sitio |

Cuatro objetivos (O1 entender rápido, O2 aprender, O3 detectar candidatos, O4 reutilizar y distribuir), recorridos con puntos de fricción y 12 casos de uso con criterios de aceptación.

> [!WARNING]
> **Todo ese análisis es hipótesis.** No hay entrevistas ni analítica (el visor no recoge ninguna, a propósito). El documento 23 propone cómo validarlo con 5 a 8 personas por tipo.

**Hallazgos útiles del análisis**
- Dos secciones del Prisma (huella espectral y fondo del cielo) están **ocultas** en el visor, aunque las notas de la v0.4.0 las nombran.
- **No hay flujo para reportar un candidato**: los comentarios quedan solo en el navegador.
- El **aviso de saturación** es la historia más urgente.

Ver [21](../documentacion/21_TIPOS_DE_USUARIO.md), [22](../documentacion/22_MAPA_DE_FUNCIONES.md), [23](../documentacion/23_RECORRIDOS_Y_OBJETIVOS.md), [24](../documentacion/24_CASOS_DE_USO.md).

## 13. Herramientas, lenguajes, datos e inteligencia artificial

| Categoría | Qué se usó |
|---|---|
| Lenguajes | Python 3.11, TypeScript, Markdown, YAML; Rust y el entorno Android vienen con Tauri y Capacitor **[Confirmar]** |
| Backend | FastAPI, astropy, HTTP Range para leer solo la parte del archivo que se necesita |
| Frontend | React 18, Vite, PWA, Aladin Lite 3.8.2 |
| Nativo | Tauri 2 (Windows, macOS, Linux), Capacitor 6 (Android) |
| Despliegue | Vercel (frontend y API), GitHub Actions para los releases, GitHub Pages |
| Datos | IRSA/IPAC (SIA2), SIMBAD, Gaia, CatWISE |
| Pruebas | pytest (46, sin red), ruff, `tsc --noEmit` |

### Uso de IA, dicho con claridad
- El **equipo** usó asistentes de IA con instrucciones del proyecto, subagentes y comandos propios. Está documentado en `AGENTS.md` y `CLAUDE.md`.
- **Yo** usé Claude Code para revisar, programar la versión única, redactar la documentación y generar gráficos SVG. Las decisiones de qué hacer y qué no, y la verificación contra el código real, fueron humanas.
- Una consecuencia práctica: **la IA se equivocó varias veces y se corrigió** (ver sección 14). La lección es que cada afirmación pasó por una verificación.

Detalle: [10 Herramientas e IA](../documentacion/10_HERRAMIENTAS_Y_IA.md).

## 14. Errores y correcciones en el camino

Una crónica honesta incluye lo que salió mal.

| Qué pasó | Cómo se corrigió |
|---|---|
| Se contaron «24 tests» y eran 46 (pruebas parametrizadas) | Se ejecutó `pytest` y se corrigió la cifra |
| Una afirmación sobre hielos en los detectores D5/D6 estaba mal | Se corrigió |
| Celdas de la comparativa afirmaban cosas sin respaldo (por ejemplo sobre WiseView) | Se suavizaron |
| La nota del riesgo R12 estaba mal redactada | Se reescribió |
| `SECURITY.md` enlazaba a avisos privados que el repositorio no tiene activados | Se quitó el enlace |
| Gráficos con cuadrantes invertidos, etiquetas cortadas y solapamientos | Se revisaron en pantalla y se rehicieron |
| Anclas de encabezados con emoji que GitHub no resuelve | Se reemplazaron y se validaron todos los enlaces con un script (0 problemas) |
| Un recuento de cobertura de las historias de usuario salió contradictorio | Se reescribió |

Y una **decisión de proceso**: al avanzar la competencia quedó claro que no convenía seguir haciendo cambios en el repositorio oficial. Desde entonces todo mi trabajo vive en el repositorio propio.

## 15. Lo que quedó pendiente

**Del producto (del equipo)**
1. Aviso cuando un objeto satura el sensor.
2. Reconciliar el contrato de la API con el backend (#72–#79).
3. Redesplegar el backend de producción (hoy 0.0.1).
4. Pruebas del frontend y un CI más estricto (#80).
5. Probar y firmar los instaladores; probar la demo en un móvil.
6. Un flujo para reportar candidatos.

**De mi repositorio** (nada de esto lo he hecho; requiere tu decisión)
1. Elegir una licencia.
2. Activar el reporte privado de vulnerabilidades.
3. Mostrar la versión visible en la web.

Hoja de ruta propuesta: [17 Hoja de ruta](../documentacion/17_HOJA_DE_RUTA.md).

## 16. Lecciones

1. **Auditar también es construir.** Un proyecto fuerte demuestra lo que afirma.
2. **Separar lo medido de lo supuesto.** Evidencia, lectura de código e hipótesis son cosas distintas.
3. **La distribución es parte del producto.** Un instalador con versión incoherente genera desconfianza aunque la ciencia esté bien.
4. **En un hackatón, el código corre más rápido que los documentos.** La deriva es esperable; lo importante es detectarla pronto.
5. **La disciplina de proceso rinde incluso en 26 horas:** PRs, worktrees, registro de riesgos y regla de «ninguna cifra sin fuente».
6. **Trabajar dentro del alcance acordado.** Aprendí a preguntar qué cambios son bienvenidos antes de hacerlos en un repositorio ajeno.
7. **La IA acelera, pero no verifica por ti.** Cada cifra se contrastó con el código o con una ejecución.

## 17. Cómo contar esta experiencia

- **Artículo para LinkedIn (preliminar):** [articulo/LINKEDIN_BORRADOR.md](../articulo/LINKEDIN_BORRADOR.md).
- **Para el CV:** una línea honesta sería: «Auditoría de coherencia entre documentación y código, versionado automatizado a partir de etiquetas de git, publicación verificable (SHA-256) y documentación técnica de un visor de datos del telescopio SPHEREx (NASA Space Apps 2026, equipo Transientes)».
- **Qué no decir:** que construí el visor; que hay usuarios validados; que los instaladores están probados.

## 18. Apéndices

### A. Glosario mínimo
- **SPHEREx:** telescopio espacial de la NASA que mapea el cielo en infrarrojo cercano.
- **IRSA / SIA2:** archivo de datos y protocolo de consulta de imágenes.
- **LVF:** filtro lineal variable; la longitud de onda cambia a lo largo del detector.
- **Blink / resta A − B:** técnicas para ver lo que cambia entre dos fechas.
- **Saturación:** cuando un objeto muy brillante llena el sensor y puede producir un movimiento falso.
- **SHA-256:** huella de un archivo que permite comprobar su integridad.
- **PWA:** página web instalable que funciona como una app.

Más términos: [11 Glosario](../documentacion/11_GLOSARIO.md).

### B. Enlaces
- Repositorio público con releases y documentación: https://github.com/vladimiracunadev-create/spherex-releases
- Página del equipo: https://www.spaceappschallenge.org/2026/find-a-team/transientes/
- Sitio del desafío: https://www.spaceappschallenge.org/

### C. Mapa de la documentación
Índice completo en [documentacion/README.md](../documentacion/README.md), con rutas de lectura según lo que busques.

---

*Este libro es un borrador. Revísalo, corrige lo que esté mal y pide al equipo que confirme lo marcado con **[Confirmar]** antes de compartirlo.*
