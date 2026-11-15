# ✅ 24 · Casos de uso e historias de usuario

Convierte el análisis de [21 Tipos de usuario](21_TIPOS_DE_USUARIO.md) y [22 Mapa de funciones](22_MAPA_DE_FUNCIONES.md) en **casos de uso con criterios de aceptación**: qué tiene que pasar para decir que algo funciona. Sirven para probar la herramienta, para una demo y para decidir qué corregir primero.

> [!NOTE]
> **Cómo leer el estado.** «Respaldado por prueba» significa que una de las 46 pruebas del backend (que ejecuté, pasan) cubre ese comportamiento. «Por lectura» significa que lo vi en el código y no lo operé. «Medido por el equipo» son cifras de los documentos del equipo que no reproduje. «Pendiente» es algo que no existe aún. Los criterios con cifras vienen de [08 Hallazgos](08_HALLAZGOS_CIENTIFICOS.md).

## 📋 Resumen

| Caso | Actor | Resumen | Estado |
|---|---|---|---|
| [UC-01](#uc-01--ver-el-movimiento-de-un-objeto-de-prueba) | U1 U6 | Ver moverse a Epsilon Indi B | 🟡 Por lectura y medido |
| [UC-02](#uc-02--ver-a-wise-0855) | U2 U3 U4 | Ver a WISE 0855 con la configuración correcta | 🟡 Medido por el equipo |
| [UC-03](#uc-03--mirar-unas-coordenadas-cualesquiera) | U3 U4 U5 | Escribir coordenadas y ver la zona | 🟡 Parcial |
| [UC-04](#uc-04--compartir-lo-que-estoy-viendo) | todas | Copiar un enlace que reproduzca la vista | 🟡 Parcial |
| [UC-05](#uc-05--completar-el-modo-misión) | U2 U1 | Recorrer los 5 niveles | 🟡 Por lectura |
| [UC-06](#uc-06--ver-el-espectro-de-un-punto) | U4 U5 | Espectro al vuelo, con advertencia | ✅ Respaldado por prueba |
| [UC-07](#uc-07--instalar-la-app-en-el-computador-o-android) | U6 U7 | Descargar, verificar e instalar | ✅ Comprobado por dentro |
| [UC-08](#uc-08--usar-la-app-en-iphone-o-ipad) | U1 | Instalar la web desde Safari | ⛔ Sin probar |
| [UC-09](#uc-09--integrar-la-api) | U7 | Llamar a la API desde otro proyecto | 🟡 Con diferencias |
| [UC-10](#uc-10--cuando-irsa-falla) | todas | Qué pasa si IRSA no responde | ✅ Respaldado por prueba |
| [UC-11](#uc-11--avisar-cuando-un-objeto-satura) | U1 U2 U3 U4 | El visor avisa de la saturación | ⛔ Pendiente |
| [UC-12](#uc-12--cambiar-de-idioma) | todas | Español e inglés | 🟡 Por lectura |

---

## UC-01 · Ver el movimiento de un objeto de prueba

| | |
|---|---|
| **Actor** | U1 Curiosa o curioso, U6 Jurado |
| **Disparador** | Abre la aplicación y quiere ver qué hace |
| **Precondiciones** | Conexión a internet; el objeto de prueba Epsilon Indi B (abre por defecto) |
| **Flujo principal** | 1. Entra en la portada. 2. Toca «Empecemos con un tutorial» o «Empecemos». 3. En la vista simple ve la imagen con el objeto marcado. 4. Elige la fecha más reciente y la más antigua. 5. Usa «Compara dos fechas» (A − B). |
| **Alternativas** | Si la imagen no carga: «Reintentar». Si quiere más herramientas: interruptor «Modo ciencia». |
| **Postcondición** | La persona ve un par rojo/azul que indica hacia dónde se movió |

**Criterios de aceptación**
- [ ] Los cinco objetos de prueba cargan **sin esperar a IRSA** (datos precalculados) → respaldado por `test_frames_de_los_5_campos_sin_red` y `test_epochs_de_los_5_campos_sin_red`.
- [ ] El visor centra en la **posición propagada de hoy**, no en la J2000 → ver UC-03.
- [ ] La resta de la última contra la primera fecha muestra un par rojo/azul. En eps Ind B el desplazamiento medido es −0,85 / −0,71 px (oeste / norte) frente a −0,92 / −0,57 px predicho → medido por el equipo.
- [ ] La persona puede **explicar con sus palabras** qué indica el par → no se ha probado con nadie (ver [23](23_RECORRIDOS_Y_OBJETIVOS.md)).

## UC-02 · Ver a WISE 0855

| | |
|---|---|
| **Actor** | U2 Docente, U3 Ciencia ciudadana, U4 Aficionado |
| **Disparador** | Quiere ver el objeto «obligatorio» del desafío, que es muy débil |
| **Flujo principal** | 1. Elige WISE 0855. 2. El visor abre con banda D5 y mediana de 10 exposiciones por visita. 3. Resta de la última visita (mayo 2026) contra la primera (mayo 2025). |
| **Alternativa** | Con una sola exposición por visita **no se ve**: es lo esperado y el texto del visor debe decirlo |

**Criterios de aceptación**
- [ ] Aparece una pareja azul (al este) y roja (al oeste), separadas ~1 píxel → medido por el equipo (0,95 px O · 0,25 px N con 10 exposiciones, frente a 1,33 px O · 0,11 px N esperados).
- [ ] El visor explica por qué se usa la mediana de 10 (`presetRazon`) → por lectura.
- [ ] No se afirma que se vea en una exposición suelta.

## UC-03 · Mirar unas coordenadas cualesquiera

| | |
|---|---|
| **Actor** | U3, U4, U5 |
| **Flujo principal** | 1. Escribe A.R./Dec en grados o sexagesimal. 2. Elige banda, rango de fechas y tamaño (0,05° a 0,5°). 3. El visor trae las épocas y muestra los fotogramas a medida que llegan. |
| **Alternativas** | Posición sin datos → estado vacío. IRSA lenta → fotogramas parciales (`pending`) y nuevas peticiones. |

**Criterios de aceptación**
- [ ] Acepta grados («133.795», «133,795») y sexagesimal, y muestra la conversión → por lectura.
- [ ] Un `size` mayor que 0,5° se **rechaza con 422** en `/api/frames` → respaldado por `test_frames_rechaza_size_mayor_al_limite`.
- [ ] Si se pide una coordenada J2000 de un objeto de prueba, el visor **avisa que centró en la posición propagada** → respaldado por `test_j2000_avisa_que_se_centro_en_la_posicion_propagada`.
- [ ] Una coordenada al azar sin conexión **falla con un mensaje claro** → respaldado por `test_coordenada_al_azar_cae_al_flujo_vivo_y_falla_claro_offline`.
- [ ] El tiempo de respuesta es razonable → **no hay umbral definido**; un recorte tarda 7,3 s de mediana (p90 10,8 s).

## UC-04 · Compartir lo que estoy viendo

| | |
|---|---|
| **Actor** | Todas |
| **Flujo principal** | 1. Ajusta posición, tamaño y banda. 2. Copia la dirección del navegador. 3. Otra persona la abre y ve lo mismo. |

**Criterios de aceptación**
- [ ] La URL guarda `ra`, `dec`, `size`, `band` y la vista → por lectura (`state/url.ts`).
- [ ] Un enlace viejo con `size=1` se recorta a 0,5° en lugar de fallar → por lectura.
- [ ] Cada nivel del Modo Misión tiene su enlace (`?mision=1…5`) → por lectura.
- [ ] **La URL guarda también el estiramiento, el modo y los cuadros por segundo** → ⛔ no: pendiente ([#74](https://github.com/mauriciovalenzuela742/SPHEREX/issues/74)).
- [ ] `?target=nombre` resuelve un nombre → ⛔ no existe ([#72](https://github.com/mauriciovalenzuela742/SPHEREX/issues/72)).

## UC-05 · Completar el Modo Misión

| | |
|---|---|
| **Actor** | U2 Estudiante o docente; U1 |
| **Flujo principal** | 1. Abre `?mision=intro`. 2. Lee la introducción (1930, el parpadeo). 3. Resuelve los niveles 1 a 5 (Barnard, WISE 0855, eps Ind B, RW Aur, R Draconis). 4. Llega al cierre «¡Lo lograste!». |

**Criterios de aceptación**
- [ ] Cada nivel carga fotos reales del objeto con su configuración (banda, tamaño, exposiciones por visita) → por lectura (`content/mision.ts`).
- [ ] El glosario explica los términos al pasar el mouse **y con el teclado** → por lectura.
- [ ] El nivel 4 («nivel de la duda») hace ver que no todo par rojo/azul es movimiento → por lectura; relacionado con UC-11.
- [ ] Se puede copiar el enlace de un nivel → por lectura.

## UC-06 · Ver el espectro de un punto

| | |
|---|---|
| **Actor** | U4 Aficionado, U5 Investigación |
| **Flujo principal** | Elige un punto o un objeto; el visor (o `/api/spectrum`) devuelve un punto (λ, flujo) por exposición de nivel 2. |

**Criterios de aceptación**
- [ ] La respuesta incluye **siempre** `caveat` (valores indicativos) → por lectura y contrato.
- [ ] Una lista de bandas inválida se rechaza **sin consultar IRSA** → respaldado por `test_spectrum_valida_bandas_sin_consultar_irsa`.
- [ ] La fotometría recupera el flujo de una gaussiana conocida → respaldado por `test_fotometria_recupera_el_flujo_de_una_gaussiana`.
- [ ] Una apertura saturada se rechaza → respaldado por `test_fotometria_rechaza_apertura_saturada`.
- [ ] El visor **no** presenta el espectro como calibrado → por lectura.

## UC-07 · Instalar la app en el computador o Android

| | |
|---|---|
| **Actor** | U6, U7 |
| **Flujo principal** | 1. Abre «Descargas» (la web o la página del repositorio). 2. Elige su plataforma (la suya aparece primero). 3. Descarga. 4. Verifica el SHA-256. 5. Instala y abre. |

**Criterios de aceptación**
- [ ] La vista «Descargas» muestra la última versión y enlaces de archivos del release → **comprobado** (vi `v0.4.2` en producción).
- [ ] Cada descarga responde 200 → **comprobado** (8 enlaces de v0.4.2).
- [ ] La suma SHA-256 del archivo coincide con la publicada → **comprobado** con el APK; el resto, no.
- [ ] La versión interna del instalador coincide con la etiqueta → **comprobado** con APK, `.exe`, `.msi` y `.deb` (0.4.2).
- [ ] Abre y funciona en un equipo real → ⛔ **sin probar**.

## UC-08 · Usar la app en iPhone o iPad

| | |
|---|---|
| **Actor** | U1 |
| **Flujo principal** | Abre el sitio en Safari → Compartir → «Añadir a pantalla de inicio». |

**Criterios de aceptación**
- [ ] La vista «Descargas» muestra la guía de 3 pasos para quien entra desde un iPhone → por lectura.
- [ ] Queda con su ícono y a pantalla completa → ⛔ **sin probar**: nadie la ha instalado en un iPhone real.
- [ ] El *service worker* funciona en Safari → ⛔ sin probar.

## UC-09 · Integrar la API

| | |
|---|---|
| **Actor** | U7 Integrador |
| **Flujo principal** | 1. Lee [03](03_API.md). 2. Llama a `/api/epochs`. 3. Llama a `/api/frames` y maneja `pending`. 4. Construye enlaces del visor. |

**Criterios de aceptación**
- [ ] `/api/epochs` respeta el contrato → respaldado por `test_api_epochs_respeta_el_contrato` y `test_api_epochs_filtra_por_fecha`.
- [ ] `/api/frames` acepta los parámetros que el contrato documenta → ⛔ **no**: `band=all`, `cmap` y estiramientos `sqrt`, `log`, `zscale` dan 422 ([#73](https://github.com/mauriciovalenzuela742/SPHEREX/issues/73)).
- [ ] Las rutas que usa el visor (`/api/grid`, `/api/frame`) están en el contrato → ⛔ no ([#75](https://github.com/mauriciovalenzuela742/SPHEREX/issues/75)).
- [ ] Una época sin banda no se pierde en silencio → ⛔ no ([#76](https://github.com/mauriciovalenzuela742/SPHEREX/issues/76)).
- [ ] La API desplegada corresponde a `main` → ⛔ no se pudo asegurar: producción decía 0.0.1 ([#77](https://github.com/mauriciovalenzuela742/SPHEREX/issues/77)).

## UC-10 · Cuando IRSA falla

| | |
|---|---|
| **Actor** | Todas |
| **Disparador** | IRSA responde 503 o tarda demasiado |

**Criterios de aceptación**
- [ ] Los cinco objetos de prueba **siguen funcionando** sin IRSA → respaldado por las pruebas de `test_demo.py`.
- [ ] Sin conexión y sin caché, el error es **explícito y claro**, no un fallo mudo → respaldado por `test_offline_sin_cache_falla_claro`.
- [ ] Un recorte ya descargado **no se vuelve a bajar** → respaldado por `test_cutout_se_descarga_una_sola_vez`.
- [ ] La interfaz ofrece «Reintentar» y mensajes legibles → por lectura.
- [ ] Los catálogos muestran su estado (cargando, N objetos, no respondió) → por lectura.

## UC-11 · Avisar cuando un objeto satura

| | |
|---|---|
| **Actor** | U1, U2, U3, U4 |
| **Por qué importa** | En Barnard y RW Aurigae el núcleo satura y la resta muestra un par rojo/azul **aunque no se muevan**. RW Aur «se mueve» 1,4 px sin moverse ([08](08_HALLAZGOS_CIENTIFICOS.md)). |

**Criterios de aceptación (propuestos)**
- [ ] Cuando más de un umbral de píxeles del núcleo está marcado como malo, el visor lo **dice** junto a la imagen.
- [ ] El texto explica en una frase por qué el par puede no ser movimiento.
- [ ] Los niveles 1 y 4 del Modo Misión (Barnard y RW Aur) lo muestran.

**Estado:** ⛔ **pendiente**. El riesgo está registrado (R13) y la hoja de ruta lo incluye ([17](17_HOJA_DE_RUTA.md)). El Modo Misión sí lo explica en sus textos.

## UC-12 · Cambiar de idioma

| | |
|---|---|
| **Actor** | Todas |
| **Flujo principal** | Toca «ES» o «EN» en el encabezado. |

**Criterios de aceptación**
- [ ] El idioma se recuerda entre visitas (`localStorage`) → por lectura.
- [ ] Textos de interfaz, tutorial y errores aparecen en el idioma elegido → por lectura; **no revisé todos los textos**.
- [ ] Un texto sin traducir no rompe la pantalla → sin comprobar.

---

## 📖 Historias de usuario

Formato: *Como [persona] quiero [algo] para [beneficio]*. La prioridad es una propuesta mía.

| # | Historia | Persona | Prioridad | Estado |
|---|---|---|---|---|
| H1 | Como curiosa quiero ver moverse una estrella en un minuto para entender de qué se trata | U1 | Alta | 🟡 UC-01 |
| H2 | Como curiosa quiero que me avisen si lo que veo puede ser un engaño para no sacar una conclusión falsa | U1 U2 | **Alta** | ⛔ UC-11 |
| H3 | Como docente quiero compartir un nivel con mi curso para trabajarlo en clase | U2 | Media | 🟡 UC-05 |
| H4 | Como docente quiero una guía de apoyo para preparar la clase | U2 | Media | ⛔ No existe |
| H5 | Como voluntaria quiero recorrer zonas al azar para encontrar candidatos | U3 | Media | 🟡 Modo ciencia |
| H6 | Como voluntaria quiero registrar y compartir un candidato para que alguien lo revise | U3 | Media | ⛔ No existe |
| H7 | Como aficionado quiero restar y apilar visitas para ver objetos débiles | U4 | Alta | 🟡 UC-02 |
| H8 | Como aficionado quiero guardar exactamente lo que veo en un enlace | U4 U5 | Media | 🟡 UC-04 (parcial) |
| H9 | Como investigadora quiero saber qué tan fiable es un espectro para decidir si lo uso | U5 | Alta | ✅ UC-06 (`caveat`) |
| H10 | Como investigadora quiero pedir fotogramas a longitud de onda igualada para comparar fechas sin mezclar colores | U5 | Media | ⛔ [17](17_HOJA_DE_RUTA.md) |
| H11 | Como jurado quiero abrir la demo desde un móvil con datos y ver algo en segundos | U6 | **Alta** | 🟡 UC-01, UC-10 |
| H12 | Como jurado quiero comprobar de dónde sale cada cifra para confiar | U6 | Alta | ✅ [Documentación](README.md) |
| H13 | Como integrador quiero un contrato de API que coincida con el código para no perder tiempo | U7 | **Alta** | ⛔ UC-09 |
| H14 | Como integrador quiero instaladores verificables para distribuirlos con confianza | U7 | Media | ✅ UC-07 (sumas) |
| H15 | Como persona en iPhone quiero instalar la app para usarla como cualquier otra | U1 | Media | ⛔ UC-08 sin probar |

## 🧮 Cobertura de un vistazo

| Estado | Casos de uso | Historias |
|---|---|---|
| ✅ Cubierto y respaldado | 3 (UC-06, UC-07, UC-10) | 3 (H9, H12, H14) |
| 🟡 Parcial o por lectura | 7 (UC-01, 02, 03, 04, 05, 09, 12) | 6 (H1, H3, H5, H7, H8, H11) |
| ⛔ Pendiente o sin probar | 2 (UC-08, UC-11) | 6 (H2, H4, H6, H10, H13, H15) |

**Lo más urgente según este análisis:** las historias de prioridad Alta que hoy no están cubiertas del todo son H1, H2, H7, H11 y H13. Por riesgo de que el producto diga algo falso o falle ante quien lo evalúa, empezaría por **H2** (avisar la saturación), **H13** (contrato de la API) y **H11** (que la demo cargue desde un móvil).

Ver también [23 Recorridos y objetivos](23_RECORRIDOS_Y_OBJETIVOS.md) y [17 Hoja de ruta](17_HOJA_DE_RUTA.md).
