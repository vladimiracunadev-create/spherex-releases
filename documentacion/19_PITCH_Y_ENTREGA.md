# 🎤 19 · El pitch y la entrega

Resumen de `docs/06_JURADO_Y_PITCH.md`, `docs/08_REGLAS_DEL_EVENTO.md` y `docs/pitch/` del repositorio oficial. Sirve para entender cómo se presentó y se entregó el proyecto, y como plantilla para otro hackatón.

## 🏆 Cómo se juzga el Space Apps

El embudo del evento tiene tres pasos: un **jurado local** elige a los *Global Nominees*; expertos de la NASA y de agencias socias eligen a los *Global Finalists* y menciones honrosas; y un comité ejecutivo elige a los **10 Global Winners**.

El primer filtro es un jurado local, con poco tiempo por equipo y sin leer el código. Después aparece gente que sí entiende de astronomía. **El proyecto tiene que funcionar en los dos registros:** impactar en dos minutos y resistir una mirada experta.

| Criterio | Qué miran |
|---|---|
| Impacto | ¿A cuánta gente le sirve y para qué? |
| Creatividad | ¿Qué hace distinto? |
| Validez | ¿Lo que muestra es cierto? |
| Relevancia | ¿Responde al desafío? |
| Presentación | ¿Se entiende en dos minutos? |

Formato en Santiago: **2 minutos de pitch + 2 preguntas**.

## 📌 El mensaje central

> Cualquiera escribe unas coordenadas del cielo y ve esa zona como un videíto, con imágenes reales de SPHEREx; además de ver qué se mueve, ve de qué está hecho.

La diferencia frente a lo que existe es el **segundo eje**: el espectro de 102 bandas. → [16 Comparativa](16_COMPARATIVA_Y_ESTADO_DEL_ARTE.md)

## 🎬 Estructura del pitch de dos minutos

El equipo preparó un guion principal y uno alternativo, con demo de 30 segundos y video. La secuencia del guion alternativo, resumida:

1. **Gancho:** cualquiera entra y ve una foto de SPHEREx: Epsilon Indi B, una enana marrón cercana.
2. **La prueba de movimiento:** primavera de 2025 y octubre de 2026; se corrió apenas un píxel. Al restar las dos fechas, rojo donde estaba y azul donde está.
3. **Las herramientas científicas:** con un interruptor aparece la misma resta, con la mediana de diez fotos por visita.
4. **El segundo eje:** con los 102 colores se arma el espectro; tiene huecos donde absorben el agua y el metano, y la herramienta lo lee: parece una enana marrón.
5. **Cierre:** cualquier estrella tiene historia; la de Barnard, en 2000, en 2010 y hoy, siempre avanzando.

## 🧱 Las reglas del pitch

- **La demo corre sola** y se narra con subtítulos; nunca se depende de una consulta fría a IRSA.
- **Todo lo que se dice tiene fuente.** Ninguna cifra sin origen comprobable.
- **Honestidad como ventaja:** decir cuándo un objeto satura o cuándo se ve débil suma más de lo que resta.
- **Probar la URL desde un móvil con datos**, no desde el wifi de la sala.
- **Nada de menores** en fotos ni voces de la demo.

## ❓ Las preguntas incómodas

| Pregunta del jurado | Respuesta del equipo (resumida) |
|---|---|
| ¿Esto no lo hace ya el Data Explorer de IRSA? | No para este público: es un archivo para astrónomos; aquí se resuelve el nombre, se agrupan las épocas, se normaliza y se entrega el parpadeo. Es la diferencia entre un archivo y un visor |
| ¿Cómo sé que la imagen es de la coordenada que pedí? | Por los marcadores de catálogo (Gaia/CatWISE/SIMBAD) a la fecha de la imagen, que caen sobre las fuentes |
| ¿Por qué no usaron aprendizaje automático? | El desafío pide un visor para personas y el cuello de botella real es traer y alinear los datos, no reconocer el movimiento. La IA aportaría en el cribado de candidatos (modelo: ALeRCE), planteado como paso siguiente |
| ¿Qué pasa si pongo una coordenada sin datos? | Se muestra un estado vacío que dice cuántas épocas hay y sugiere un campo cercano |
| ¿Esto escala a todo el cielo? | La consulta es por posición, así que sí en cobertura. Lo que no escala sin infraestructura es la caché: habría que precomputar recortes, como hace unWISE para WiseView |
| ¿Qué tan correctos son los espectros? | Indicativos: fotometría de apertura con la luz zodiacal restada, y se dice en la interfaz. Para ciencia, la herramienta de IRSA. **Nunca decir que son de calidad científica** |

Más respuestas en [14 Preguntas y respuestas](14_PREGUNTAS_FRECUENTES.md).

## 📋 Lo obligatorio de la entrega

| Requisito | Detalle |
|---|---|
| **Qué se sube** | Nombre y resumen del proyecto, enlace a la demo final, datos de la NASA y otras fuentes usadas, y cómo se usó IA |
| **Declaración de uso de IA** | Obligatoria este año. Escrita bien, es evidencia de método. Ver [10](10_HERRAMIENTAS_Y_IA.md) |
| **Atribución de SPHEREx** | Literal, en el pie de la herramienta y en el README ([04](04_DATOS_SPHEREX.md)) |
| **Enlaces que funcionen desde fuera** | Probados desde un móvil con datos |
| **Datos** | Abiertos de la NASA y de sus agencias socias |
| **Cierre** | Domingo 15-nov-2026, 11:00 (hora de Santiago) |

Esas reglas se comunicaron en vivo por la organización y el equipo no las contrastó con la página oficial; **si difieren de la plataforma, manda la plataforma.**

## 🔁 Qué repetir y qué mejorar en otro hackatón

**Repetir**
- Fijar un contrato de API en la hora 1 para trabajar en paralelo.
- Medir antes de decidir (el estudio de datos cambió la arquitectura: sin CORS, sin caché no hay demo).
- Tener un plan B visible (cinco campos precalculados y un video).
- Exigir «qué NO comprobé» en cada cambio.

**Mejorar**
- Mantener el documento del contrato al día con el código: la auditoría halló diez diferencias.
- Medir el despliegue completo antes del cierre: el backend quedó en la versión 0.0.1.
- Probar con personas ajenas al tema, y en el dispositivo real (iPhone).
- Dejar la versión de los instaladores atada a la etiqueta desde el principio.

## 🔗 Recursos del proyecto

- Aplicación: <https://spherex-planet-x-tau.vercel.app/>
- Descargas: <https://vladimiracunadev-create.github.io/spherex-releases/>
- Desafío: <https://www.spaceappschallenge.org/2026/challenges/planet-x-and-spherex/>
- Evento: <https://www.spaceappschallenge.org/>
