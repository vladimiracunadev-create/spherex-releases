# ✅ 12 · Instalar y verificar una descarga

Cada release de este repositorio trae un archivo de sumas SHA-256 (`SHA256SUMS.txt` en el último release; `SHA256SUMS-vX.Y.Z.txt` en los anteriores). Sirve para comprobar que lo que descargaste es **idéntico** al archivo publicado.

> [!CAUTION]
> **Antes de instalar:** los instaladores no están firmados y no se probaron en equipos reales. Descárgalos solo de este repositorio o del [oficial](https://github.com/mauriciovalenzuela742/SPHEREX/releases) y verifica la suma.

## 🎯 Qué archivo elegir

| Tengo… | Descargo |
|---|---|
| 🪟 Windows 10/11 | `AstroTrack-windows-…-setup.exe` (o el `.msi`) |
| 🍎 Mac (Intel o Apple Silicon) | `AstroTrack-macos-…universal.dmg` |
| 🐧 Linux (cualquiera) | `AstroTrack-linux-…AppImage` |
| 🐧 Linux Debian o Ubuntu | `AstroTrack-linux-…amd64.deb` |
| 🤖 Android | `AstroTrack-android-vX.Y.Z.apk` |
| 📱 iPhone o iPad | Nada que descargar: abrir <https://spherex-planet-x-tau.vercel.app> en Safari → Compartir → «Añadir a pantalla de inicio» |
| 🌐 Un servidor propio | `spherex-pwa-vX.Y.Z.zip` (build estático; necesita los *rewrites* de `/api` y `/proto-api` de `vercel.json`) |

## 🔐 Comprobar la suma

Descarga también el archivo de sumas y deja ambos en la misma carpeta.

**Windows (PowerShell)** — calcula la suma y compárala a ojo con la línea del archivo de sumas:

```powershell
Get-FileHash .\AstroTrack-android-v0.4.2.apk -Algorithm SHA256
```

**macOS**

```bash
shasum -a 256 AstroTrack-macos-AstroTrack_0.4.2_universal.dmg
```

**Linux** — comprueba todos los archivos a la vez; cada línea debe decir `OK`:

```bash
sha256sum -c SHA256SUMS.txt --ignore-missing
```

Si una suma no coincide, **no instales el archivo**: bórralo y vuelve a descargarlo.

## 🚦 Primera ejecución

| Sistema | Qué verás | Qué hacer |
|---|---|---|
| 🪟 Windows | «Windows protegió su PC» (SmartScreen) | *Más información* → *Ejecutar de todos modos* |
| 🍎 macOS | Gatekeeper bloquea la app | Clic derecho sobre la app → *Abrir* |
| 🤖 Android | Pide permiso para instalar apps desconocidas | Permitirlo para tu navegador; ábrelo y vuelve a desactivarlo después |
| 🐧 Linux (AppImage) | No se ejecuta | Dar permiso: `chmod +x AstroTrack-*.AppImage` |
| 🐧 Linux (.deb) | — | `sudo apt install ./AstroTrack-*.deb` |

La aplicación abre el sitio publicado, por lo que **necesita internet** y siempre muestra la última versión del visor, independientemente del número del instalador.

## 🔢 ¿Qué versión tengo?

- Desde **v0.4.2** el número del instalador coincide con el del release (Windows lo muestra en *Propiedades → Detalles*; Android en *Información de la app*).
- Las versiones **v0.1.0 a v0.4.1** dicen 0.2.0 en los instaladores de escritorio y 1.0 en el APK; es solo etiquetado.

## 🧹 Desinstalar

Windows: *Configuración → Aplicaciones*. macOS: arrastrar la app a la papelera. Linux: `sudo apt remove astro-track` (el paquete `.deb` se llama `astro-track`) o borrar el AppImage. Android: mantener pulsado el ícono → *Desinstalar*. PWA: quitar el ícono de la pantalla de inicio.
