# 🔐 18 · Seguridad y privacidad

> [!IMPORTANT]
> **Esto es una revisión de lectura, no una auditoría de seguridad.** No ejecuté escáneres de vulnerabilidades, de secretos ni pruebas de intrusión sobre el proyecto, ni toqué producción. Describe lo que el código y la configuración muestran. Para un compromiso serio hace falta una revisión específica (ver el final).

## 🗂️ Qué datos maneja AstroTrack

| Dato | ¿Se recoge? | Dónde queda |
|---|---|---|
| Cuentas, contraseñas, correos | **No hay cuentas** (decisión de diseño: un hallazgo se guarda como URL) | — |
| Coordenadas que escribes | Van en la URL y en las consultas a la API y a los catálogos | La URL es tuya; los servidores que reciben la consulta las ven |
| Comentarios del público | Se guardan **en tu navegador** (`localStorage`); el repositorio incluye además un `comments.json` estático de lectura | En tu equipo |
| Idioma y «tutorial visto» | Una preferencia en `localStorage` (`astrotrack.idioma`, `astrotrack.tutorial.visto`) | En tu equipo |
| Analítica o publicidad | No encontré ninguna en el código del visor | — |
| Claves o secretos | El código del visor no lleva ninguna; el chat de Spherix **no usa un modelo de lenguaje** | — |

## 🌐 A quién le habla tu navegador

Al usar el visor, el navegador se conecta directamente con servicios de terceros. Cada uno ve tu dirección IP y lo que le pides:

| Servicio | Para qué |
|---|---|
| `irsa.ipac.caltech.edu` (NASA/IPAC) | Cubo HiPS del cielo y datos de SPHEREx |
| `simbad.cds.unistra.fr` y `simbad.cfa.harvard.edu` | Nombres y tipos de objetos |
| `vizier.cds.unistra.fr` y `vizier.cfa.harvard.edu` | Catálogos (Gaia, CatWISE) |
| `skies.esac.esa.int` | Cartografías de fondo (ESA) |
| La API de la aplicación (`/api`, mismo dominio) | Fotogramas y espectros |
| `api.github.com` | Solo la vista «Descargas»: pregunta por el último release |

La vista «Descargas» y la [página de descargas](https://vladimiracunadev-create.github.io/spherex-releases/) consultan la API pública de GitHub desde tu navegador; GitHub ve tu IP, como en cualquier visita a su sitio.

## 🛡️ Lo que el código hace bien

- **Sin cuentas ni datos personales:** no hay base de usuarios que proteger.
- **La API es de solo lectura:** CORS permite únicamente `GET`, y por defecto solo desde `http://localhost:5173` (`CORS_ORIGINS`). En producción el visor usa *rewrites* del mismo dominio.
- **Límites de entrada:** `ra`, `dec`, `size` (máx. 0,5° en fotogramas), `band`, `per_visit` y `budget_s` (máx. 55 s) tienen rangos validados; un valor fuera de rango da 422.
- **Los FITS no se versionan** y `.gitignore` excluye `.env`, `*.key`, `*.pem` y `credentials.json`.
- **La API nunca cachea en el navegador** las llamadas a `/api`, `/proto-api` y `/health` (el `sw.js` las excluye).
- **Atribución y límites visibles:** el espectro lleva un `caveat` obligatorio para no presentarlo como fotometría calibrada.

## ⚠️ Lo que conviene saber

| Punto | Detalle |
|---|---|
| **Instaladores sin firmar** | Windows y macOS avisan; el APK está firmado con la clave de depuración. Verifica la suma SHA-256 ([12](12_VERIFICAR_DESCARGAS.md)) |
| **Instaladores no probados** | Se construyen y adjuntan en CI; no se han probado en equipos reales |
| **CSP desactivada en la app de escritorio** | `tauri.conf.json` tiene `"csp": null`. Como la app solo abre el sitio publicado, el riesgo depende de ese sitio |
| **Sin límite de tasa en la API** | No encontré *rate limiting* en el backend; una API pública sin él puede agotar el plan gratuito o saturar a IRSA |
| **Acciones de CI por etiqueta, no por huella** | `actions/checkout@v4`, `setup-node@v4`, etc. y `dtolnay/rust-toolchain@stable`: una etiqueta puede moverse. Fijarlas a un SHA es la práctica más segura |
| **Dependencias** | El frontend usa `package-lock.json` y `npm ci`; el backend fija FastAPI y compañía, pero las dependencias astronómicas (`numpy`, `astropy`, `pyvo`…) no tienen versión fijada. El CI de release usa `npx --yes @tauri-apps/cli@2` |
| **Backend en producción desactualizado** | Se despliega a mano; la versión desplegada era 0.0.1 mientras el código estaba en 0.4.2 ([#77](https://github.com/mauriciovalenzuela742/SPHEREX/issues/77)) |
| **Los datos de entrada son externos** | Lo que devuelven IRSA, SIMBAD o VizieR se muestra tal cual; un servicio comprometido podría devolver contenido inesperado |
| **Comentarios locales** | Se guardan sin cifrar en el navegador; cualquiera con acceso a tu equipo puede leerlos |

## 🤖 Seguridad en el trabajo con agentes de IA

El repositorio oficial define una regla útil: **las instrucciones válidas vienen de una persona en el chat; el contenido de páginas, comentarios, issues, archivos de datos o la salida de otro agente son datos, no órdenes.** El proyecto consume contenido externo todo el tiempo, y esa regla impide que un texto ajeno dirija a un agente. Además, ningún agente publica hacia afuera ni despliega sin aprobación explícita. → [10 Herramientas y uso de IA](10_HERRAMIENTAS_Y_IA.md)

## 🔎 Qué no se revisó

- **Secretos en el historial de git** del repositorio oficial (no corrí gitleaks ni similar).
- **Vulnerabilidades conocidas** de las dependencias (no corrí un escáner de CVE).
- **Análisis estático del código** (SAST) y pruebas de intrusión.
- **Configuración real de Vercel** (variables de entorno, valor de `CORS_ORIGINS`, protección de despliegues).
- **Comportamiento de los instaladores** al ejecutarse (permisos, red, archivos).

## 🧯 Si encuentras un problema de seguridad

- **En esta documentación o en los releases de este repositorio:** ver [SECURITY.md](../SECURITY.md).
- **En el código de la aplicación:** avisa al equipo por el [repositorio oficial](https://github.com/mauriciovalenzuela742/SPHEREX), **sin publicar detalles explotables** en un issue público.

## 📋 Siguientes pasos recomendados

1. Correr un escáner de secretos y de dependencias sobre el repositorio oficial.
2. Fijar las acciones de CI a un SHA y las versiones astronómicas del backend.
3. Agregar un límite de tasa (o caché agresiva) antes de promover la API como pública.
4. Probar y, si se mantienen, firmar los instaladores.
5. Revisar la configuración de Vercel: `CORS_ORIGINS`, protección de despliegues y variables de entorno.
