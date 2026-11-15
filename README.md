# spherex-releases

Copia pública de los instaladores de **AstroTrack · SPHEREx** (NASA Space Apps 2026, desafío *Planet X and SPHEREx*).
No contiene código fuente: solo la página de enlaces y los releases.

- Aplicación web (en línea): https://spherex-planet-x-tau.vercel.app/
- Página con los enlaces oficiales: https://vladimiracunadev-create.github.io/spherex-releases/
- Releases anteriores (v0.1.0 a v0.4.1) también están aquí, con sus sumas.
- Release vigente: [v0.4.2](https://github.com/vladimiracunadev-create/spherex-releases/releases/tag/v0.4.2)
- Origen: release `v0.4.1` del repositorio oficial del equipo (etiqueta `v0.4.2`). Los archivos son idénticos; `SHA256SUMS.txt` permite comprobarlo.

Los instaladores no están firmados y no se probaron en equipos reales. Publicado el 2026-11-15.

## Mi participación

Proyecto de equipo (6 personas) del hackatón NASA Space Apps 2026. El repositorio del código es del equipo
([mauriciovalenzuela742/SPHEREX](https://github.com/mauriciovalenzuela742/SPHEREX)); este repositorio solo reúne las descargas. Mi parte:

- **Acceso a datos (rol D1):** consulta a IRSA (SIA2) con caché en disco, el endpoint `/api/epochs`, reintentos ante errores 502 y el precálculo de los 5 campos de la demo para servirlos sin IRSA en vivo.
- **Empaquetado y entrega:** interfaz móvil y PWA instalable ([#43](https://github.com/mauriciovalenzuela742/SPHEREX/pull/43)); instaladores de escritorio con Tauri y APK de Android con Capacitor ([#44](https://github.com/mauriciovalenzuela742/SPHEREX/pull/44)); release automático en GitHub Actions al empujar una etiqueta.
- **Vista «Descargas» de la web** ([#84](https://github.com/mauriciovalenzuela742/SPHEREX/pull/84)): botones por plataforma, detección del dispositivo e instrucciones de instalación.
- **Versión única** ([#86](https://github.com/mauriciovalenzuela742/SPHEREX/pull/86)): la versión del instalador y del APK sale de la etiqueta del release (v0.4.2 es el primero consistente) y el producto pasa a llamarse AstroTrack.
- **Auditoría de coherencia** entre documentación y código ([#71](https://github.com/mauriciovalenzuela742/SPHEREX/pull/71)): 10 hallazgos abiertos como issues [#72 a #81](https://github.com/mauriciovalenzuela742/SPHEREX/issues), con 46 pruebas del backend ejecutadas.
- **Este repositorio:** copia pública de los 7 releases (v0.1.0 a v0.4.2) con sumas SHA-256 y una página que toma la última versión automáticamente.

**Alcance y límites.** El visor, el diseño y el resto del backend son trabajo de otras personas del equipo. Los
instaladores no están firmados y no se probaron en equipos reales. Parte del código de las piezas de entrega y de
la auditoría se escribió con asistencia de IA (Claude Code) y fue revisado y publicado por mí. Esto describe mi
contribución, no un resultado del hackatón.
