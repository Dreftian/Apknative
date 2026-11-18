# Guía de contribución

Agradecemos tus problemas (issues) y solicitudes de extracción (pull requests). El principio fundamental de **Apknative** es: **entorno ultraligero sin dependencias externas y comportamiento predecible**. Lee las siguientes pautas antes de comenzar.

## Entorno de desarrollo

- Windows 10/11 x64
- Node.js ≥ 20
- `npm install && npm start`

## Comprobación previa al envío

```powershell
npm test          # Pruebas unitarias de Node + pruebas de enlace del motor (sin conexión a Internet ni VM)
```

Al realizar cambios en la interfaz de usuario, se recomienda ejecutar una captura de pantalla de comprobación rápida:

```powershell
electron . --smoke test\shot.png
```

## Convenciones de código

- **El contenedor principal no introduce dependencias de tiempo de ejecución de terceros**. Los módulos en `src/core/` para ZIP / AXML / ARSC / PNG / ICO están implementados desde cero. Al agregar funciones, prioriza los módulos nativos de Node.js.
- La capa de presentación utiliza HTML5/CSS/JavaScript vanilla nativo, sin frameworks externos.
- La comunicación con Android se realiza exclusivamente a través de `src/core/adb.js`; no concatenes comandos `adb` directamente.
- La comunicación entre el proceso principal y los procesos de renderizado se realiza solo a través de los canales permitidos expuestos por `preload.js`, manteniendo `contextIsolation: true`.
- Los identificadores de código deben estar en inglés; las cadenas orientadas al usuario se traducen al español.

## Convenciones de prueba

- Cuando no se detecte un archivo APK real, las pruebas omitirán automáticamente el análisis de APK. Para cubrirlas, define la variable de entorno:

```powershell
$env:APKSTART_TEST_APK="C:\ruta\a\cualquier.apk"; npm test
```

- No realices peticiones a Internet en las pruebas. Aquellas que requieran comportamiento de red deben iniciar un servidor local `http` (ver `test/engine-test.js`).

## Mensajes de confirmación (Commits)

Utiliza el estándar Conventional Commits, por ejemplo:

```
feat(engine): soporte para importar disco qcow2 desde ruta personalizada
fix(axml): corregir desplazamiento base en la tabla de atributos
docs: actualizar instrucciones de habilitación de WHPX
```

## Aspectos legales y límites

- Este proyecto no distribuye imágenes del sistema Android, binarios de QEMU ni componentes propietarios de Google; todos son descargados según sea necesario por el usuario desde fuentes oficiales.
- No envíes APKs comerciales, herramientas de craqueo ni material que infrinja derechos de autor de terceros al repositorio.
