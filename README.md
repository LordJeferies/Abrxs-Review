# Abrxs Review · instalación y Drive

Página estática para revisión y fichas. No necesita npm, modelos ni servidor privado. Incluye los archivos HTML, CSS y JS: hay que subir **todos**, no solo index.html. GitHub Pages sirve la interfaz pública; no aloja tus videos de Drive.

## Configurar Google una sola vez

1. En https://console.cloud.google.com/ crea un proyecto y habilita **Google Drive API**.
2. En Google Auth Platform configura nombre, correo y audiencia. Para uso personal, déjalo en pruebas y añade tu cuenta como usuario de prueba. Google puede exigir volver a autorizar sesiones; el acceso no es permanente.
3. Crea un cliente OAuth de tipo **Aplicación web**. En orígenes JavaScript autorizados añade `https://lordjeferies.github.io` y, para probar en Mac, `http://localhost:8765`. El origen no lleva `/nombre-del-repo/`. No necesitas secreto de cliente, cuenta de servicio ni clave API en esta página.
4. Copia el **ID público**, terminado en `.apps.googleusercontent.com`. En la página abre Configurar conexión a Drive, pégalo, pulsa Preparar inicio de sesión y después Iniciar sesión en Drive. Las dos acciones evitan abrir una ventana de Google fuera del gesto del usuario.
5. Autoriza lectura y pega el enlace de tu carpeta. También puedes navegar desde Mi Drive. No se cambia ningún archivo de Drive.

Esta versión usa `drive.readonly`: permite leer archivos de la cuenta, no solo una carpeta. La UI navega por la carpeta elegida, pero **el permiso es amplio**. Se eligió así porque `drive.file` con Picker no concede automáticamente acceso a todos los hijos de una carpeta. Solo conecta una cuenta que quieras autorizar. Para distribuir la conexión a otras personas, revisa los requisitos de verificación de Google para scopes restringidos; publicar el HTML no completa esa verificación.

Fuentes oficiales: https://developers.google.com/workspace/drive/api/quickstart/js · https://developers.google.com/identity/oauth2/web/guides/use-token-model · https://developers.google.com/workspace/drive/api/guides/api-specific-auth

## Publicar en GitHub Pages (cuando tú decidas)

Sube el contenido de esta carpeta a la raíz de un repositorio dedicado, por ejemplo `Abrxs-Review`. No subas videos, transcripciones, copias de proyecto, registros ni secretos. En Settings → Pages selecciona Deploy from a branch, main, /(root). La dirección será `https://lordjeferies.github.io/Abrxs-Review/`. No cambia el repositorio de la app. Puedes alojarlo en una subcarpeta también: las rutas son relativas.

En iPhone abre la dirección en Safari → Compartir → Añadir a pantalla de inicio → abrir como app web. Si Google bloquea la ventana desde la app instalada, abre la misma dirección en Safari y conecta allí. Son entornos que pueden guardar borradores separados: exporta una copia para transferirlos. No se ha probado todavía tu sesión OAuth ni un iPhone real.

## Usar

- Conecta Drive y elige un video, o abre un video del dispositivo. El streaming privado usa peticiones parciales; el token solo vive en memoria. Si el navegador suspende el reproductor, reconecta y selecciona de nuevo el video. Nunca se añade el token al enlace.
- Si el streaming falla, Cargar clip completo descarga a memoria únicamente videos de hasta 150 MB y pide confirmación. Para másteres grandes, descarga desde Drive y abre como archivo local. MP4 H.264/AAC es la opción de mayor compatibilidad; no se convierte el video desde esta página.
- Marca ahora, escribe comentario y guarda. Los tiempos de notas pertenecen al **archivo revisado**. Un montaje reordenado no tiene una conversión simple al máster. No se inventa esa relación.
- Carga JSON `words`/`segments`, SRT, VTT o TXT `[00:00:10.000 --> 00:00:15.000] texto`. Sin tiempos no puede seleccionarse con precisión. Si solo hay tiempos por frase, la selección es por frase, no por palabra ficticia.
- Escribe el nombre exacto del máster. Toca el primer y el último fragmento, añade bloque, crea otras fichas y ordena con ↑ ↓. Ajusta los tiempos de cada bloque para empezar antes o terminar después. La referencia reproduce solo ese intervalo si has abierto el máster con ese nombre; debes comprobar que sea el archivo correcto.
- Exporta Cortes TXT/JSON. Ambos contienen el mismo JSON editorial compatible con el importador de Mac. Selecciona **el mismo máster** allí. El importador actual añade normalmente 0.2 s de margen: revisa antes de renderizar. Las notas TXT/JSON son documentos de revisión, **no órdenes de corte automáticas**.
- Guardar proyecto descarga una copia completa con notas, transcripción y fichas. No contiene videos ni tokens. Abrir copia recupera el trabajo. El borrador del navegador puede perderse si borras datos, cambia el origen o iOS libera almacenamiento: descarga copias regularmente.

## Privacidad y límites

Sin analítica, anuncios ni envío de notas a un servicio propio. El inicio de sesión contacta Google y Drive; Google aplica sus políticas. El ID OAuth público se guarda localmente; el token nunca se guarda en localStorage ni en caché. El service worker guarda solamente los archivos públicos de interfaz. No hay sincronización automática ni escritura en Drive. La página no transcribe, analiza con IA ni renderiza: entrega decisiones a la app local. El mapa no es un editor de efectos ni una imagen generada.

La interfaz utiliza formas, bordes y textura de puntos creados con CSS/SVG. No hay una imagen de interfaz fingiendo controles funcionales. Se adaptó el patrón OAuth del repositorio oficial `googleworkspace/browser-samples` (Apache-2.0); se conserva su licencia. Se evitó copiar el repositorio completo o añadir una librería de arrastre: los controles ↑ ↓ funcionan también con touch y teclado.
