# Changelog de Orbital Music

Todas las novedades y correcciones de Orbital Music, de la más reciente a la más antigua.

*Este changelog se construyó fusionando el Registro de todos los cambios y el CHANGELOG.md del proyecto.*

---

## [2026-09-25] - Sistema de actualizaciones por GitHub Releases

### Agregado
- **Actualizaciones automáticas por GitHub Releases**: la app consulta el release más reciente de un repo público de binarios, descarga e instala la nueva versión sola. Adiós al JSON en Drive y a MediaFire.
- Nuevo repo público de binarios `IrvinCraft/Orbital-Music-Releases` con el release **v1.0.0** publicado (instalador del CI).
- **Build del instalador .exe automático en GitHub Actions** (Windows) — artefacto descargable en cada build.
- Script `tools/build-exe.ps1` para compilar el instalador .exe en Windows de forma local.
- En Windows los logs viven siempre en `Documentos/OrbitalMusic/logs` (fácil de compartir por red SMB), incluso con el .exe instalado; el logging queda activo siempre en el instalador.

### Corregido
- Eliminado el menú manual de prueba de FFmpeg en Ajustes (el instalador automático de FFmpeg ya funciona solo).
- Probada la detección de actualizaciones de punta a punta: un release de prueba v1.0.1 fue detectado correctamente (y eliminado después).

## [2026-09-23] - La canción ya no se reinicia tras un corte de red

### Corregido
- Si el streaming se corta (fallo de red/TLS), al reintentar la canción **reanuda desde la posición** donde murió, en vez de empezar de cero.

## [2026-09-20] - Estabilidad del Home y congelado real en segundo plano

### Corregido
- El Home ya no "desaparece" por una falsa desconexión: se declara offline solo tras **2 sondas fallidas seguidas**, no por un timeout puntual.
- Al cerrar la app en segundo plano se restaura la **posición real** de la canción (antes se guardaba una posición congelada).
- El marquee (título/artista) ya no sigue animando en segundo plano.
- El log de sesión `.txt` vuelve a generarse (el logging quedó activo por defecto).

## [2026-09-19] - Segundo plano sin render, FFmpeg dentro del instalador

### Agregado
- En segundo plano la app muestra el **último frame congelado** en vez de pantalla negra, con **cero rendering** (niri/Hyprland y al minimizar).
- **FFmpeg empaquetado dentro** del instalador .exe y del AppImage: la app ya no depende de descargas externas al instalar. En Arch se usa el FFmpeg del sistema (pacman).
- Seguimiento de primer plano configurable por plataforma: bloqueado en Windows (ahí basta minimizar); en Linux solo se activa con Hyprland/niri.
- Compilador unificado `compilar.sh` (paquete Arch, instalador Windows, AppImage) y compilación automática en GitHub Actions.
- Menú en Ajustes para probar la descarga de FFmpeg (retirado el 25-09, ya no hace falta).

### Corregido
- La app ya no se redibuja a ~4fps en segundo plano (el ticker de posición se congela en background).
- Descarga automática de FFmpeg arreglada: URLs dinámicas vía API de BtbN + barra de progreso real.
- Linux ya no depende de VLC (solo FFmpeg); quitada la dependencia `vlc` del paquete Arch.
- Crash al abrir la versión compilada (AppImage/pacman) corregido (NPE) y MPRIS funcionando en paquetes.
- AppImage real de nuevo: `build-appimage.sh` genera un `.AppImage` verdadero.

## [2026-09-16] - Restauración de estado y optimizaciones

### Agregado
- Flags de compilación por plataforma (`-PtargetOS=windows|linux`): cada build incluye solo su código (SMTC en Windows; MPRIS en Linux).
- Optimizaciones de rendimiento: eliminados 4 cuellos de botella (serialización de cola, afinidad de artistas, deduplicación del Home, logs de Discord).

### Corregido
- Todas las pantallas **restauran su estado** al volver de minimizar o cambiar de pestaña (búsqueda, vistas, scrolls, ajustes).
- Paquete Arch: corregido el build que usaba el jar en vez de la distribución completa.

## [2026-09-15] - IA local de recomendaciones (Taste Engine) y fixes

### Agregado
- **Orbital Taste Engine**: perfil de gusto que aprende de tus escuchas — secciones **"Para ti"**, **"Artistas que te pueden gustar"**, **"De artistas afines"** y **"Similar a X"** mejoradas, con decaimiento temporal y caché de 24h.
- **Sistema de puntos transparente**: bono de click (+1.0), bono de búsqueda (+0.25), bono de like (+3.0), bono "quizá" (+0.50 por escuchar más de la mitad), castigo a canciones saltadas. Escuchar menos del 50% no cuenta como reproducción.
- Script `orbital_taste_admin.py`: ranking con **desglose de puntos** (por qué se obtuvo cada punto), watch en vivo, revocación del like y `reset --confirm`.
- Cobertura de logs detallada en todo el proyecto + sesión `.txt` por apertura en `logs/`.
- Portada por HTTP local en MPRIS (KDE Connect).

### Corregido
- El buscador mostraba solo 1 resultado (Top result) → ahora devuelve **todas las secciones** (canciones, videos, álbumes, artistas, playlists).
- KDE Connect (Linux): el teléfono mostraba `0:00` y portada gris → ahora tiempo, seek y portada funcionan.
- Seek desde controles externos (Hyprland/MPRIS/KDE Connect): sin doble reproducción, sin saltos.
- El bono de click ya no se aplica a canciones de radio/auto-avance.
- La recencia se gana solo con escuchas reales (eliminados los "puntos de fábrica").

## [2026-09-11] - Motor de recomendaciones propio y soporte Wayland

### Agregado
- **Orbital Taste Engine**: semillas inteligentes, vecindarios votados y grafo de artistas para recomendaciones reales.
- "Sigue escuchando" con **columnas adaptativas** al ancho de la ventana.
- Barra de título propia con botones maximizar/cerrar para **Hyprland y niri** (Linux Wayland).

### Corregido
- Imágenes de perfil en "Artistas que te pueden gustar".

## [2026-08-01] - Radio, Chromecast y Almacenamiento

### Agregado
- Radio: prioriza canciones similares del **mismo género**.
- Chromecast: descubrimiento en Linux usando la **interfaz de red correcta**.
- Ajustes de Almacenamiento: estadísticas en vivo de caché/descargas, **límites separados por categoría** con barra de uso, switch y botones de limpieza independientes.

## [2026-07-19] - Soporte táctil y fixes de reproducción

### Agregado
- Soporte táctil para **scroll vertical y horizontal** en todas las pantallas.

### Corregido
- Seek ya no duplica la reproducción y el volumen se aplica en tiempo real.

## [2026-07-18] - FFmpegAudioPlayer: nuevo motor de audio

### Agregado
- **FFmpegAudioPlayer**: nuevo motor de audio basado en FFmpeg + Java Sound (reemplaza a VLCJ).

### Corregido
- 5 bugs críticos de reproducción: canciones cacheadas que no sonaban, doble reproducción, barra de progreso (duración 0:00), posición reiniciada al reanudar y duración incorrecta entre sesiones.

## [2026-07-17] - Atribución y control del proyecto

### Agregado
- Atribución de código (Metrolist / Orbital Music) y comentario de control en todos los archivos.
- Archivos de lyrics actualizados desde la versión de referencia.
- Git hook pre-commit que exige actualizar el registro de cambios.

---

## [v1.2.0] - 2026-07-15

### Agregado
- **Reproducción Offline (Home)**: Cuando no hay internet, la pantalla de inicio muestra automáticamente las canciones descargadas y en caché en secciones "Descargas" y "Canciones en caché". Al reconectar, restaura las recomendaciones online automáticamente.
- **VISIONOS como Cliente de Streaming**: Nuevo cliente `VISIONOS` disponible en Configuración > Streaming. Emula un dispositivo Apple Vision OS para resolución de streams, con soporte completo en Desktop y Android.
- **Selección Manual de Cliente**: En Configuración > Streaming, puedes elegir un cliente específico (ANDROID_VR, WEB_REMIX, VISIONOS) o AUTO. Cuando seleccionas uno manualmente, solo se intenta ese cliente. En AUTO se prueba la cadena completa priorizando ANDROID_VR.
- **Refresco Automático de Sesión al Reconectar**: Al detectar reconexión WiFi, el sistema refresca automáticamente el `visitorData` y limpia la caché de descifrado de NewPipe para evitar bloqueos "Accede para confirmar que no eres un bot".

### Corregido
- **Race Condition en Reconexión WiFi**: Se corrigió un bug crítico donde `fetchRecommendations()` desde el observer de Auth reseteaba `isShowingLocalContent = false`, pisando el valor del offline handler. Esto impedía que el Home se actualizara al reconectar.
- **Tolerancia a streams "UNPLAYABLE"**: El reproductor ya no filtra por estado `playabilityStatus`. Intenta todos los clientes disponibles y registra en logs por qué cada uno falla, maximizando las posibilidades de reproducción.
- **Prioridad de Cliente de Streaming**: `ANDROID_VR` ahora es el cliente prioritario en DesktopAudioPlayer, DownloadManager y YTPlayerUtils (Android), reemplazando a `WEB_REMIX` como primer intento.
- **HEAD requests eliminados**: Se removió la validación HEAD previa a la descarga de streams que causaba falsos positivos 403.

### Rendimiento
- **Limpieza de Archivos Muertos**: Eliminados `src/`, `build_local/` (vacíos), `ComposeDebugUtils.kt` (no usado), función `md5()` en `StringUtils.kt` (no llamada), logs JVM (`hs_err_pid*`, `replay_pid*`), y `__pycache__/` (artefacto Python residual).
- **Protocolo Listen Together centralizado**: `Protocol.kt` (327 líneas, antes duplicado en app y desktop) movido al módulo compartido `innertube` para eliminar redundancia.

## [v1.1.1] - 2026-01-29

### Corregido
- **Fix Maestro de Personalización ("No more Cocomelon")**:
    - Sincronización total con la lógica de Android (v12.12.3) para la gestión de sesiones.
    - Captura avanzada de `VISITOR_DATA` y `DATASYNC_ID` durante el login para una personalización real.
    - Implementación de `visitorData` autenticado: las recomendaciones ahora reflejan fielmente el historial y gustos del usuario.
    - Se requiere cerrar sesión y volver a entrar una sola vez para activar la personalización completa.
- **Estabilidad de Reproducción (CÓDIGO ROJO)**:
    - Priorización de clientes de alto rendimiento (`ANDROID_VR`) para evitar errores 403 y 400.
    - Limpieza automática de caché de descifrado para asegurar el inicio inmediato de streams.
    - Mejoras en el manejo de User-Agents para evitar bloqueos de YouTube Music.
- **Seguridad y Privacidad**:
    - **Encriptación de Tokens**: Implementada seguridad AES-128 vinculada al hardware para proteger sesiones de YouTube y Discord.
    - **Limpieza de Seguridad**: Al actualizar, se cerrarán las sesiones previas una única vez para eliminar credenciales en texto plano por seguridad de todos.
- **Modo Rendimiento**: Nueva opción en Configuración > Reproducción para dispositivos de bajos recursos. Desactiva animaciones pesadas (Disco Background) ahorrando hasta un 30% de CPU/GPU.

## [v1.1.0] - 2026-01-25

### Agregado
- **Sistema Avanzado de Letras**:
    - **Nuevos Proveedores**: Integración de **SimpMusic** (VideoID preciso) y **KuGou** (Base de datos masiva) junto a BetterLyrics y LrcLib.
    - **Configuración de Prioridad**: Nuevo panel de configuración para activar/desactivar proveedores y elegir tu favorito.
    - **Auto-Upgrade Inteligente**: El sistema reemplaza automáticamente las letras estáticas (Time 0) por sincronizadas si encuentra una mejor versión en segundo plano.
    - **Overlay Mejorado**: Barras de progreso y volumen integradas en la vista de letras, con colores dinámicos adaptados a la carátula.
- **Cerebro de Recomendaciones Híbrido**: Nuevo motor local que aprende de tus gustos para ofrecer recomendaciones inteligentes offline.
- **Historial Local Inteligente**: Deduplicación diaria y sincronización precisa con YouTube.

### Corregido
- **Restauración de Sesión**: Las letras ahora se cargan automáticamente al reabrir la app y restaurar la última canción.
- **Fix Maestro de Portadas Locales**: Rediseño del motor de carga de imágenes para evitar errores en Windows.
- **Interfaz de Ajustes**: Correcciones de espaciado y diseño visual en la nueva pantalla de Letras.

## [v1.0.9] - 2026-01-21

### Agregado
- **Integración con Windows SMTC (System Media Transport Controls)**:
    - **Control Total de Teclas**: Soporte completo para teclas multimedia de hardware (Play, Pause, Next, Previous).
    - **Visual Overlay**: Visualización de **título, artista y carátula del álbum** en la superposición de volumen/medios de Windows.
    - **Sincronización de Barra de Progreso**: La posición de reproducción y la duración se sincronizan en tiempo real con el sistema.
- **Mejoras Visuales Premium**:
    - **Texto en Movimiento (Marquee)**: Los títulos largos ahora se desplazan suavemente en el reproductor y en las tarjetas del "Home", eliminando el texto cortado.
    - **Animaciones Interactivas**: Implementación de efectos de escala (Hover & Press) en tarjetas y Quick Picks para una respuesta táctil y visual de alta gama.
- **Rediseño Completo de Biblioteca**:
    - **Nueva Vista de Detalle**: Implementación de `LibraryDetailView`, una interfaz moderna y unificada para "Mis Me Gusta", "Descargas" y "Caché".
    - **Hero Header Dinámico**: Cabecera visual impactante con carátula reciente, estadísticas y botones de acciones rápidas (Shuffle/Play All).
- **Optimización de Almacenamiento**:
    - **Limpieza Inteligente**: Eliminación automática de caché (audio y carátula) al marcar canciones como "No me interesa".
- **Rediseño del Motor de Recomendaciones (Home)**:
    - **Algoritmo "Similar a" Corregido**: Uso de la API `YouTube.related` para descubrimientos reales.
    - **Favoritos Olvidados**: Nueva sección automática para redescubrir canciones de tu biblioteca poco escuchadas.
    - **Deduplicación Inteligente**: Filtro avanzado para evitar versiones duplicadas de la misma canción en el feed.
- **Modos de Reproducción Avanzados**:
    - **Control de Volumen VLC-Style**: Rango extendido hasta **125%** con ajustes finos.
    - **Sincronización de Historial**: Registro mejorado de reproducciones para mejores recomendaciones futuras.

### Corregido
- **Optimización de SMTC**: Eliminado el retraso en la carga de carátulas mediante un sistema de detección proactiva.
- **Estabilidad de Reproducción Crítica**: Auto-reintento inteligente con rotación de clientes (ANDROID/WEB) y aumento de buffer a 3000ms.
- **Sincronización Optimista**: Los "Me gusta" se reflejan instantáneamente en la interfaz.
- **Limpieza de Interfaz**:
    - **Filtro de Playlists**: Ocultación de playlists automáticas redundantes de YouTube (como "Liked Music" o "Episodes for later") para dejar espacio solo a las playlists creadas por el usuario.
    - **Mejora en Búsqueda**: El icono de flecha en la barra de búsqueda fue reemplazado por una "X" más intuitiva para cerrar el buscador.
    - **Orden Inteligente**: Las listas de Descargas y Caché ahora aparecen ordenadas por fecha de modificación (lo más reciente arriba).

## [v1.0.8] - 2026-01-18

### Corregido
- **Caché Inteligente (Corrección de Lógica)**: Se solucionó un error crítico donde las canciones con duración desconocida (0s) se descargaban erróneamente. Ahora, si la duración es 0, la app consulta a la API de YouTube para verificar la longitud real antes de descargar.
- **Estabilidad de Reproducción (VLC)**:
    - **Prioridad de Cliente**: Se cambió la estrategia de resolución de streams para priorizar el cliente `ANDROID` (más estable) sobre `WEB_REMIX`, eliminando errores de reproducción al iniciar sesión.
    - **User-Agent Dinámico**: VLC ahora recibe el User-Agent exacto del cliente que resolvió el stream, evitando errores 403.
    - **Auto-Skip**: Implementación de un sistema de salto automático que avanza a la siguiente canción tras 2 segundos de error, evitando que la música se detenga.
- **Discord Rich Presence**:
    - **Branding**: El icono pequeño ahora muestra correctamente el logo de "Orbital Music" en lugar de duplicar la portada.
    - **Fallback**: Se corrigió la lógica de imagen de respaldo para evitar mostrar logos antiguos ("Metrolist").
- **Radio Infinito**: La radio ahora carga automáticamente más canciones cuando quedan 5 o menos, proporcionando reproducción continua sin límites. Además, las canciones se mezclan aleatoriamente para mayor variedad.
- **Playback Tracking**: Las reproducciones ahora se registran en YouTube Music, permitiendo que las recomendaciones y el historial reflejen lo que escuchas en Orbital Music.

## [v1.0.7] - 2026-01-14

### Agregado
- **Optimización de Velocidad**: Reducción drástica del tiempo de inicio de canciones. Las canciones locales ahora inician de forma instantánea gracias a la optimización de `:file-caching` y `:network-caching` en el motor VLC.
- **Caché Inteligente**: Implementación de lógica para omitir la descarga automática de pistas de más de 30 minutos (Podcasts/DJ Mixes), optimizando el almacenamiento.
- **Ajustes de Reproducción**: Nueva tarjeta de configuración que permite activar manualmente la descarga de canciones largas.
- **Mejoras Visuales (Full Player)**:
    - Título y artista ahora utilizan el esquema de colores dinámico de la portada.
    - **Corrección de Contraste**: El botón de reproducción ahora detecta si la carátula es muy clara (blanca) y cambia el color del icono a oscuro automáticamente para garantizar visibilidad total.
- **Discord Rich Presence**: Corrección de la barra de progreso que no se mostraba correctamente; sincronización mejorada de `startTime` y `endTime`.
- **Fortificación Offline**:
    - Las carátulas ahora cargan desde el almacenamiento local cuando estás sin internet.
    - Se añadió un mensaje de "Sin conexión a internet" en la sección de Likes.
- **Auto-Recuperación de Conexión**: Nuevo detector de red que recarga automáticamente tus "Me Gusta" y re-conecta Discord al recuperar el internet.
- **SettingsManager**: Nueva arquitectura para el guardado persistente de preferencias de la aplicación.

## [v1.0.6] - 2026-01-14

### Agregado
- **Optimización de CPU**: Implementación de un modo de ahorro de energía que se activa automáticamente al minimizar la app.
- **Throttling Inteligente**: Reducción de la frecuencia de actualización de posición (de 100ms a 5s) cuando la ventana no es visible.
- **Sincronización Eficiente de Discord**: Las actualizaciones ahora son puramente **basadas en eventos** (cambio de pista o pausa), eliminando el tráfico de red innecesario mientras se mantiene la barra de progreso fluida en Discord.
- **Detección de Ventana**: Nuevo `WindowStateManager` para gestionar estados de la interfaz de forma centralizada.
- **Optimización Gráfica**: El visualizador de música se detiene automáticamente en segundo plano para ahorrar recursos de GPU.
- **Persistencia de Reproducción**:
    - Guarda automáticamente la última canción escuchada, la posición exacta y la cola de reproducción al cerrar la app.
    - Restaura el estado en modo pausa al reiniciar para una experiencia fluida.
    - Se persistió también la duración de la pista para asegurar que las barras de progreso se muestren correctamente desde el inicio.
    - Optimización del motor VLC para evitar parpadeos visuales durante la restauración mediante el uso de parámetros nativos `:start-time`.
- **Optimización de RAM**: Reducción del heap de la JVM de Gradle a 1GB y optimización de los daemons para mitigar el alto consumo de memoria del sistema.
- **Correcciones de Estabilidad**: Resolución de errores de Skiko (`UnsatisfiedLinkError`) mediante la estandarización de versiones de Compose y limpieza de caché de librerías nativas.

## [v1.0.5] - 2026-01-13

### Agregado
- **Seguridad Avanzada**: Sistema de validación de HUID (HardwareID) vinculado a la cuenta del usuario.
- **Protección contra Piratería**: El ejecutable queda bloqueado automáticamente si se detecta que los archivos han sido movidos a otro PC sin autorización.
- **Mensajes de ToS Profesionales**: Notificación clara de violación de términos de servicio si el hardware no coincide.
- **Panel Administrativo (CLI)**: Nueva herramienta `admin_cli.py` para gestionar pro-activamente usuarios y baneos.
- **Persistencia de Seguridad**: Caché de validación de 30 días para permitir el uso offline sin comprometer la seguridad.

## [v1.0.4] - 2026-01-13 (Hotfix)

### Corregido
- **Error de Instalador**: Resolución de problemas en la generación del archivo `.msi`.
- **Icono de Sistema**: Corrección del icono en la barra de tareas de Windows.

## [v1.0.3] - 2026-01-12

### Agregado
- **Independencia Total de VLC**: Integración de librerías nativas (`vlcj-natives-binaries`) mediante JitPack para máxima portabilidad.
- **Flujo de Actualización Seguro**: Separación de descarga e instalación. Ahora se muestra una barra de progreso real y un botón de "Instalar Ahora" manual.
- **Sistema de Beta Testers**: Se habilitó el acceso para probadores externos, incluyendo a `Noe Ivan` (Beta Tester #1).
- **Login Interactivo y Transicional**: Nueva pantalla de bloqueo con campos de Usuario y Contraseña, incluyendo una transición de bienvenida personalizada con el rango del usuario y una animación de carga ("spinner").
- **Roles de Usuario**: Implementación de etiquetas dinámicas para identificar a desarrolladores (`AI-Enhanced Developer`) y probadores.
- **Detección Inteligente de Audio**: El programa ahora detecta si falta VLC y muestra un asistente de descarga oficial para evitar errores de reproducción.
- **Preparación v1.0.3**: Sincronización de versiones internas y del instalador para lanzamiento oficial.
- **Soporte de Mediafire**: Motor de extracción inteligente para descargar actualizaciones pesadas (>100MB) evitando bloqueos de seguridad.
- **Normalización de Enlaces**: Conversión automática de links de visualización de Drive a descarga directa.
- **Nueva Experiencia en Inicio (Home)**: Implementación de carruseles dinámicos "Shelf" y carga optimizada de secciones (Mixes, Recomendaciones, Éxitos).

### Corregido
- **Descargas Corruptas**: Solucionado el problema donde se descargaban solo 100KB en lugar del instalador completo de 150MB.
- **Estabilidad de Instalador**: El proceso de cierre de la app ahora está coordinado con el lanzamiento del instalador para evitar bloqueos de archivos.

## [v1.0.2] - 2026-01-12

### Agregado
- **Sistema de Seguridad Local (App Lock)**: Bloqueo de inicio mediante contraseña maestra para proteger el código durante el desarrollo.
- **Protección de Autoría**: Nueva sección "Acerca de" en Ajustes y cabeceras de copyright GPL v3.0 en archivos clave.
- **Archivo de Créditos**: Creación de `CREDITS.md` para reconocer a los autores del port (IrvinCraft y Antigravity).
- **Enlaces Dinámicos en Discord**: Título y artista en Discord ahora funcionan como enlaces directos a YouTube Music.
- **Throttling de Discord**: Optimización de actualizaciones para evitar desconexiones y saturación del gateway.
- **Limpieza de Carátulas**: Servidor de imágenes optimizado (`img.youtube.com`) para solucionar el icono de interrogación (?).

### Corregido
- **Modo Aleatorio en Playlists**: Solucionado el error donde las canciones se seleccionaban pero no sonaban.
- **Latencia de Inicio**: La música ahora empieza a sonar al instante mediante streaming mientras se cachea en segundo plano.
- **Compatibilidad de Streaming**: Se añadió soporte de `playlistId` para autorizar pistas restringidas de YouTube.

## [v1.0.1] - 2026-01-11

### Agregado
- **Integración con Discord Rich Presence**: Visualización de música en el perfil de Discord.
- **Persistencia de Configuración**: Guardado automático de tokens y preferencias de Discord.
- **Icono de Aplicación**: Actualización del icono nativo de Windows (ic_launcher-playstore.png).

## [v1.0.0] - 2026-01-10

### Agregado
- **Port Inicial a Desktop**: Primera versión funcional para Windows.
- **AuthManager**: Sistema de login mediante cookies de YouTube Music.
- **Navegación Desktop**: Implementación de barra lateral y pantallas adaptadas.
- **Soporte de Audio**: Motor de reproducción compatible con Windows.

---

*Mantenido por IrvinCraft y AI-Enhanced Developer.*