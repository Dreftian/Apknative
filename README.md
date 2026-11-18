# Apknative

<p align="center"><img src="docs/images/logo.png" alt="Logo de Apknative" width="160" /></p>

[![CI](https://github.com/Dreftian/Apknative/actions/workflows/ci.yml/badge.svg)](https://github.com/Dreftian/Apknative/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/Dreftian/Apknative?include_prereleases&sort=semver)](https://github.com/Dreftian/Apknative/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-0078D6.svg)](#)

**Ejecutor de aplicaciones Android para Windows — instala y usa aplicaciones Android como si fueran software nativo de Windows.**

- 🖱️ **Doble clic en .apk para instalar**: Asocia archivos `.apk` en el Explorador de archivos de Windows; haz doble clic para analizar automáticamente → instalar → crear accesos directos en el escritorio, sin necesidad de abrir interfaces de emuladores pesados.
- 🧩 **Un icono en el escritorio por aplicación**: Los iconos se extraen directamente del APK sin conexión y se convierten en archivos ICO multirresolución. Doble clic en el icono = inicio directo de la aplicación, exactamente igual a abrir un software nativo de Windows.
- 🪟 **Cada aplicación es una ventana**: El entorno se ejecuta con ventana independiente. Apknative ajusta el título, asigna el icono auténtico de la aplicación y posiciona la ventana según la relación de aspecto vertical u horizontal. Modo **pantalla completa** centrado o modo **ventana** ajustable que recuerda el tamaño por aplicación.
- 🔍 **Búsqueda nativa en Windows**: Crea accesos directos en el Escritorio y en el Menú Inicio (`Programas\Apknative\`), permitiendo buscar apps por su nombre y agruparlas de forma independiente en la barra de tareas.
- 📁 **Intercambio bidireccional de datos**: Sincronización continua en tiempo real entre `%USERPROFILE%\ApknativeShared` y `/sdcard/ApknativeShared` en Android. Los archivos guardados en la app móvil aparecen de inmediato en tu PC.
- 🪶 **Ultraligero**: El contenedor no tiene dependencias pesadas de terceros (los analizadores de ZIP, PNG, ICO y AXML están implementados desde cero). El entorno se activa bajo demanda y se suspende en reposo para liberar memoria.

![Interfaz principal](docs/images/app.png)

```
┌────────────────────────────────────────────────────────────┐
│  Acceso directo en el escritorio (icono real .lnk)          │
│      │  Doble clic                                         │
│      ▼                                                     │
│  Apknative.exe --launch <nombre_del_paquete>               │
│      │  ¿Entorno inactivo? → Inicio automático (Android 14)│
│      ▼                                                     │
│  Lanzamiento mediante Activity LAUNCHER del sistema        │
│      │  Monitor de estabilidad y reintentos automáticos     │
│      ▼                                                     │
│  Ventana de aplicación nativa (título, icono y proporción) │
└────────────────────────────────────────────────────────────┘
```

## Descargas listas para usar

Descarga la versión más reciente desde los lanzamientos oficiales en GitHub:

- GitHub Releases: [github.com/Dreftian/Apknative/releases](https://github.com/Dreftian/Apknative/releases)

### Opciones de paquetes disponibles:

| Modalidad | Archivo | Cuándo elegirlo |
|---|---|---|
| **Tienda Windows (MSIX)** | `Apknative_<versión>_x64.msix` | Instalación limpia mediante paquete MSIX moderno. Actualizaciones automáticas y aislamiento seguro. |
| **Portable Completo** | `Apknative-v*-win64-full.zip` (~830 MB, descomprimido ~1.3 GB) | **Sin instalación ni esperas de descarga**: descomprimir y usar de inmediato. Incluye el motor y la imagen del sistema lista. |
| **Instalador estándar** | `Apknative-v*-Setup.exe` (~1.3 GB) | Instalador guiado con accesos directos en Escritorio, Menú Inicio y desinstalador en Configuración de Windows. |
| **Portable Ligero** | `Apknative-v*-win64-portable.zip` (~96 MB) | Versión ultracompacta para probar; descarga el entorno de ejecución en el primer inicio bajo demanda. |

> Todas las versiones de `Apknative.exe` incluyen **iconos oficiales y metadatos de versión integrados** (`ProductName = Apknative`, `FileDescription = Apknative Ejecutor de aplicaciones Android`).
> Para empaquetar desde el código fuente: `npm run pack` (ligero) / `npm run pack:full` (completo) / `npm run build:msix` (paquete MSIX).

### Adaptación de pantalla y diseño responsive

La orientación física del panel del emulador (`hw.lcd.width/height`) define el formato de visualización real. Si la orientación física no coincide con la declarada por la aplicación (`screenOrientation`), Android generaría barras negras laterales (letterbox).

Por ello, **Apknative analiza `AndroidManifest.xml` sin conexión al instalar cada APK**, identificando si la aplicación fue diseñada de forma nativa para vertical u horizontal:

- **Aplicaciones verticales**: Panel vertical de 720×1280, densidad lógica de 190 dpi (**606 dp**, superando el punto de corte de 600 dp de Android para habilitar diseño de tableta en dos columnas).
- **Aplicaciones horizontales**: Panel horizontal de 1280×720 a pantalla completa.

Solo si la orientación de la nueva aplicación difiere de la sesión previa se reinicia el entorno (aprox. 15 segundos); las siguientes aplicaciones de igual orientación inician al instante. Puedes alternar la orientación en cualquier momento desde la barra de herramientas o en Configuración → Opciones de ventana.

### Actualizaciones automáticas

En Configuración → **Actualización de software** → «Comprobar actualizaciones», o haz clic derecho en el icono de la bandeja del sistema. Al detectar una nueva versión, haz clic en «Actualizar ahora».

- El instalador descarga `Apknative-v*-Setup.exe` y actualiza de forma silenciosa.
- La versión portable reemplaza el directorio de la aplicación preservando tus datos, AVDs y aplicaciones instaladas en `%APPDATA%\apknative`.
- La sustitución se gestiona mediante un script autónomo de PowerShell para evitar ventanas de error o procesos bloqueados.

---

## Compilación desde el código fuente

Requisitos: Windows 10/11 x64, Node.js ≥ 20.

```powershell
# 1. Clonar el repositorio
git clone https://github.com/Dreftian/Apknative.git
cd Apknative

# 2. Instalar dependencias
npm install

# 3. Iniciar la aplicación y desplegar el entorno
npm start

# 4. (Opcional) Activar asociación de archivos .apk
npm run assoc:on

# 5. Compilar versión portable
npm run pack
# Genera dist/Apknative-win64-portable/Apknative.exe
```

### Creación del instalador completo sin conexión

```powershell
# Empaqueta y comprime el entorno de ejecución
npm run runtime

# Construye el instalador NSIS
npm run build:exe
# Genera dist/release/Apknative-<versión>-Setup.exe
```

---

## Detalles del entorno de ejecución

Apknative es completamente **autónomo** e independiente; descarga, verifica y gestiona sus componentes en `%APPDATA%\apknative\runtime`:

| Componente | Origen | Tamaño | Propósito |
|---|---|---|---|
| **adb (platform-tools)** | dl.google.com | ~8 MB | Instalación, inicio e inspección de aplicaciones |
| **Android Emulator** | dl.google.com (descarga paralela en 8 hilos) | ~440 MB | Motor de ejecución y virtualización con traducción GPU |
| **Imagen genérica Android 14** (Google Play x86_64) | dl.google.com | ~1.1 GB (comprimido en xz) | Base del sistema Android 14 (API 34) compatible con apps x86 y ARM |
| **Controlador AEHD** (opcional) | dl.google.com | ~0.2 MB | Aceleración por hardware en CPUs compatibles cuando WHPX no esté activo |

El asistente de configuración permite preparar todo con **un solo clic**:
1. Descarga e instalación automática de adb, el motor y la imagen del sistema.
2. Inicio automático en segundo plano; posteriormente, hacer doble clic en cualquier APK inicia el sistema con total fluidez.

![Asistente de despliegue](docs/images/setup.png)

### Rendimiento y arquitectura optimizada

- **Capa base de solo lectura compartida**: Todos los aplicativos comparten una única imagen del sistema en `system-images/`, ahorrando decenas de gigabytes.
- **Capa de datos ligera y diferenciada**: Almacenamiento rápido en formato ext4 con instantáneas `quickboot`. Suspender la aplicación guarda el estado de memoria para restablecer la sesión en segundos.
- **Precalentamiento en reposo (opcional)**: Inicia el entorno Android en segundo plano para que al abrir cualquier aplicación la respuesta sea inmediata.
- **Aislamiento de ventanas de terminal**: Todos los subprocesos de QEMU, `netsimd` y consolas de emulación son suprimidos a nivel de Win32, mostrando exclusivamente la interfaz de tu aplicación.
- **Modo de compatibilidad ARM**: Android 14 incluye bibliotecas avanzadas de traducción nativa (`ndk_translation`), permitiendo ejecutar aplicaciones móviles que solo contienen código compilado para procesadores ARM64/ARMv7.

---

## Estructura del proyecto

```
Apknative/
├─ src/
│  ├─ main.js              Proceso principal: ventanas, bandeja, IPC y ciclo de vida
│  ├─ preload.js           Puente seguro de contexto (ContextBridge)
│  ├─ ui/                  Interfaz de usuario (HTML5, CSS y JavaScript vanilla)
│  └─ core/
│     ├─ zip.js            Lector y extractor ZIP minimalista sin dependencias
│     ├─ axml.js           Decodificador de Android Binary XML
│     ├─ arsc.js           Analizador de recursos resources.arsc (nombres e iconos)
│     ├─ apk.js            Extracción de metadatos del APK: iconos, actividades y ABI
│     ├─ png.js            Codificador PNG y generador de iconos
│     ├─ shortcut.js       Generación de iconos ICO multirresolución y accesos .lnk
│     ├─ assoc.js          Asociación de extensiones .apk en el registro de Windows
│     ├─ adb.js            Cliente adb interno para control de la máquina virtual
│     ├─ engine.js         Gestión del ciclo de vida del emulador y procesos
│     ├─ sharer.js         Sincronización bidireccional de directorios
│     ├─ installer.js      Flujo de instalación y preparación de aplicaciones
│     └─ store.js          Persistencia de configuración y registro de aplicaciones
├─ build/                  Scripts de compilación, empaquetado y recetas NSIS/MSIX
├─ test/                   Suite completa de pruebas unitarias y de integración
└─ package.json            Manifiesto del proyecto y scripts npm
```

---

## Preguntas frecuentes (FAQ)

### ¿Cómo activar la aceleración por hardware si aparece una advertencia?

Existen dos vías de aceleración para el emulador Android en Windows:

| Método | Descripción | Requiere permisos de administrador | ¿Requiere reiniciar el PC? |
|---|---|---|---|
| **WHPX** | Plataforma de hipervisor de Windows (función nativa) | Al habilitar la característica | Sí |
| **AEHD** | Controlador de hipervisor de Google | Al instalar el controlador | No |

- **Versión Instalador / Portable**: Ve a Configuración → Autodiagnóstico del entorno → haz clic en **Reparar** junto a «Aceleración por hardware». Se solicitará confirmación UAC para activar WHPX automáticamente.
- **Versión Microsoft Store (MSIX)**: Al hacer clic en «Reparar», se abrirá la ventana de «Características de Windows». Marca la casilla **«Plataforma de hipervisor de Windows»** (HypervisorPlatform), haz clic en Aceptar y reinicia la computadora.

También puedes habilitarlo desde una consola de PowerShell con privilegios de administrador:

```powershell
dism /online /enable-feature /featurename:HypervisorPlatform /all /norestart
dism /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

### ¿Cómo restaurar los accesos directos o iconos de mis aplicaciones?

Si algún icono aparece en blanco o no refleja la imagen de la app:
1. Abre **Configuración → Integración del sistema → Reconstruir accesos directos**; esto regenerará todos los archivos `.lnk` e iconos `.ico` en el Escritorio y Menú Inicio.
2. También puedes hacer clic derecho sobre la tarjeta de la aplicación en Apknative y seleccionar **Crear acceso directo en el escritorio**.

### ¿Por qué `adb devices` en mi terminal no muestra el emulador?

El servicio adb de Apknative utiliza deliberadamente el **puerto privado 5044** para no interferir con instalaciones locales de Android Studio ni herramientas como scrcpy (que usan el puerto 5037 por defecto).

Para conectar tu consola adb al entorno de Apknative:

```powershell
$env:ANDROID_ADB_SERVER_PORT = 5044
adb devices
```

---

## Contribuciones

Las contribuciones son bienvenidas. Lee atentamente la [Guía de contribución](CONTRIBUTING.md) antes de enviar un Pull Request. Asegúrate de que todas las pruebas pasen ejecutando `npm test`.

## Licencia

Distribuido bajo la licencia [MIT](LICENSE). Copyright (c) 2026 Contribuidores de Apknative.
