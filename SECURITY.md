# Política de seguridad

Este repositorio **no contiene código fuente de la aplicación**: guarda una copia pública de los releases de AstroTrack · SPHEREx (instaladores, APK y zip de la web), sus sumas SHA-256, una página de descargas y documentación.

## Cómo comprobar que una descarga es la publicada

Calcula el SHA-256 del archivo y compáralo con `SHA256SUMS.txt` (o `SHA256SUMS-vX.Y.Z.txt`) del mismo release. Instrucciones por sistema en [`documentacion/12_VERIFICAR_DESCARGAS.md`](documentacion/12_VERIFICAR_DESCARGAS.md).

**Los instaladores no están firmados** y no se han probado en equipos reales. Una suma que coincide demuestra que el archivo es idéntico al publicado, no que el instalador sea seguro.

## Cómo reportar un problema

- **Un archivo de un release no coincide con su suma, o parece manipulado:** abre un [issue](https://github.com/vladimiracunadev-create/spherex-releases/issues/new) **sin adjuntar el archivo** y sin detalles explotables, indicando el nombre del archivo, el release y la suma que obtuviste. Este repositorio todavía no tiene activados los avisos de seguridad privados de GitHub.
- **Un error en esta documentación:** un [issue](https://github.com/vladimiracunadev-create/spherex-releases/issues) normal.
- **Una vulnerabilidad en la aplicación:** el código vive en el [repositorio oficial del equipo](https://github.com/mauriciovalenzuela742/SPHEREX); avisa allí sin publicar detalles explotables.

## Alcance

Este repositorio no tiene una versión soportada ni un compromiso de respuesta: es la copia de un proyecto de hackatón. Lo que se revisó y lo que no está descrito en [`documentacion/18_SEGURIDAD_Y_PRIVACIDAD.md`](documentacion/18_SEGURIDAD_Y_PRIVACIDAD.md).
