# Borrador de artículo para LinkedIn (preliminar, sin publicar)

> Revisa los datos entre [corchetes] antes de publicar. Ajusta la parte de «mi participación» a lo que realmente hiciste.

---

**Título:** Lo que aprendí auditando un proyecto de la NASA Space Apps: cuando el contrato y el código no dicen lo mismo

**Subtítulo:** AstroTrack · SPHEREx, un visor para ver cómo se mueve el cielo con imágenes reales del telescopio SPHEREx de la NASA

---

En el NASA Space Apps Challenge 2026 participé en un equipo que trabajó el desafío *Planet X and SPHEREx*: ¿se puede usar el archivo público del telescopio SPHEREx para detectar objetos que se mueven en el cielo?

El proyecto del equipo, AstroTrack, es un visor web (con versiones de escritorio y Android) que compara imágenes del mismo punto del cielo en fechas distintas. Si algo se movió entre una foto y otra, aparece como un par rojo y azul. Todo con datos reales del archivo IRSA de la NASA, sin cuentas ni servidores propios para los datos.

Somos el equipo **Transientes**, de Santiago de Chile: Juan Pablo Castillo, Bryan Cabezas, Antonia Del Castillo, Mauricio Valenzuela, María Javiera Valenzuela y yo. [Pide permiso a cada persona antes de publicar sus nombres.]

Yo no fui quien construyó el núcleo del visor. Mi aporte fue otro, y creo que igual de útil en una competencia con poco tiempo: **revisar si lo que el proyecto decía que hacía coincidía con lo que hacía**.

## 1. Auditar el desvío entre documentación y código

Apliqué una revisión de «deriva» (drift) sobre el repositorio del equipo y encontré 10 diferencias entre el contrato de la API y el código, por ejemplo un endpoint documentado que no existía, parámetros que no coincidían y un backend de producción desplegado a mano que iba por detrás del código. Las documenté como issues, con evidencia, para que el equipo decidiera qué corregir.

La lección: en un hackathon el código avanza más rápido que los documentos. Una auditoría corta a mitad de camino cuesta poco y evita llegar a la demo con una API que no hace lo que dice.

## 2. Una sola versión para todo

Los instaladores decían versiones distintas entre sí (0.2.0 por dentro, 1.0 en el APK). Propuse y apliqué que la versión salga **de la etiqueta de git** y se estampe automáticamente en los instaladores de escritorio y Android. Desde la v0.4.2 todo coincide y el producto se llama igual en todas partes.

## 3. Hacer que se pueda probar

Publiqué un repositorio espejo con todos los lanzamientos (v0.1.0 a v0.4.2) y sumas de verificación SHA-256, más una página que enlaza a cada descarga. Así cualquier persona del jurado puede instalar y comprobar que el archivo es el que se publicó.

## 4. Documentar para quien llega después

Escribí 25 documentos: arquitectura, datos de SPHEREx, riesgos, preguntas frecuentes, comparativa con otras herramientas, hoja de ruta y un análisis de siete tipos de usuario con casos de uso. Una decisión que defendería siempre: **todo lo que es hipótesis dice que es hipótesis**. No hay entrevistas ni analítica de uso; las métricas propuestas están marcadas como propuestas, no como resultados.

## Qué me llevo

- **Auditar también es construir.** Un proyecto sólido no es solo el que tiene más funciones, sino el que puede demostrar lo que afirma.
- **Separar lo medido de lo supuesto.** Evidencia, inspección del código e hipótesis son tres cosas distintas y conviene etiquetarlas.
- **La distribución es parte del producto.** Un instalador sin versión coherente ni verificación genera desconfianza aunque la ciencia esté bien.
- **Usé IA (Claude) como apoyo** para revisar, redactar y generar gráficos; las decisiones y la verificación de los datos fueron mías. Lo declaro porque creo que es la práctica correcta.

Qué sigue: el equipo tiene pendientes abiertos (contrato de la API, aviso cuando un objeto satura el sensor, firma de instaladores). Si te interesa la astronomía abierta, el desarrollo web o la calidad de software, me encantaría conversar.

🔗 Repositorio con releases, documentación y verificación: https://github.com/vladimiracunadev-create/spherex-releases
🔗 Desafío: https://www.spaceappschallenge.org/

#NASASpaceApps #SPHEREx #Astronomía #OpenData #Python #React #QualityAssurance #DocumentaciónTécnica #IA
