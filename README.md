# Vigilabebés Local

Un solo archivo (`index.html`) que convierte dos móviles en un vigilabebés. Sin servidor, sin cuenta y sin datos móviles: el audio viaja directo entre los dos móviles por el WiFi de casa.

- **Móvil del bebé:** escucha y solo deja pasar el sonido cuando detecta ruido. Retrasa el audio 0,3 s para no perder el principio de las palabras y mantiene el sonido 5 s tras el último ruido.
- **Móvil de padres:** reproduce el sonido, muestra el último sonido y suena una alarma si se pierde la señal.

## Publicarlo en GitHub Pages (5 minutos)

1. En github.com, crea un repositorio nuevo (por ejemplo `vigilabebes`), público.
2. Pulsa **Add file → Upload files** y sube `index.html` (y este README si quieres). Confirma con **Commit changes**.
3. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` y guarda.
4. Espera 1-2 minutos. El enlace será `https://TU-USUARIO.github.io/vigilabebes/`.
5. Abre ese enlace en los dos iPhone con Safari. Opcional: Compartir → *Añadir a pantalla de inicio*; pero para vigilar de verdad es más fiable dejarlo en una pestaña de Safari (ver pruebas).

## Cómo emparejar

Los dos móviles deben estar en el mismo WiFi y con internet (solo para encontrarse).

**La primera vez (con PIN):**
0. La primera vez que abres la página en cada móvil, elige **Móvil del bebé** o **Móvil de padres** (se recuerda; se cambia en *Ajustes → Este móvil*).
1. **Móvil del bebé:** pulsa **Crear PIN** (acepta el permiso del micrófono). Aparece un PIN de 6 cifras.
2. **Móvil de padres:** escribe el PIN → **Conectar** (acepta el permiso del micrófono; solo sirve para que Safari use la red local, no graba nada).
3. En unos segundos conectan. Los dos móviles guardan un secreto largo y el PIN ya no hace falta.

**Después (sin PIN):** el móvil de padres busca solo al bebé. Si en 10 minutos no lo encuentra (y nunca se conectó en esa sesión), deja de buscar para ahorrar batería y muestra **Buscar de nuevo**; también vuelve a buscar al reabrir la página. En el del bebé, abre la página y pulsa el círculo (**Activar**): se conectan solos. Si se corta el WiFi o se cierra una página, se reconectan solos en cuanto vuelve.

**Emparejar con otro móvil:** en *Ajustes* de cualquiera de los dos, pulsa **Emparejar con otro móvil**. Olvida el emparejamiento en los dos (si están conectados) y vuelve a pedir un PIN. Si no estaban conectados, hay que pulsarlo también en el otro.

Cómo funciona: el PIN, y después el secreto guardado, solo sirven para que los dos móviles se encuentren a través de un servicio de mensajes (por defecto ntfy.sh, gratuito y público). El audio va directo de móvil a móvil por tu WiFi y no pasa por internet.

## Servicio de emparejamiento propio (opcional, gratis)

Por defecto se usa ntfy.sh. Si prefieres no depender de un servicio de terceros, puedes crear el tuyo en Cloudflare en 10 minutos:

1. Crea una cuenta gratuita en cloudflare.com y entra en **Workers & Pages → Create → Create Worker**. Ponle un nombre (por ejemplo `vigilabebes`) y pulsa **Deploy**.
2. En **Storage & Databases → KV → Create**, crea un espacio llamado `vigilabebes-sig`.
3. En tu Worker: **Settings → Bindings → Add → KV namespace**. Nombre de variable: `SIG`. Espacio: `vigilabebes-sig`.
4. En tu Worker: **Edit code**, borra lo que haya, pega el contenido de `servicio-propio/worker.js` y pulsa **Deploy**.
5. Copia la URL del Worker (por ejemplo `https://vigilabebes.TU-USUARIO.workers.dev/`).
6. En `index.html`, busca la línea `var SIGNAL=window.VB_SIGNAL||'https://ntfy.sh/'` y cambia `https://ntfy.sh/` por tu URL, **terminando en `/`**. Sube el archivo a GitHub.

El servicio guarda cada mensaje 10 minutos y nunca ve el audio. Después de cambiarlo hay que emparejar de nuevo con un PIN.

## Historial y registro de pruebas

- **Historial (móvil de padres):** en la pantalla principal, *Historial* muestra los últimos sonidos (hora, duración y pico) y los cortes de conexión, con un resumen de las últimas 12 h. Ocupa muy poco: se guardan como máximo 200 eventos.
- **Registro de pruebas (los dos móviles):** en *Registro de pruebas* se guarda en el propio móvil lo que va pasando: micrófono, conexiones y reconexiones, sonidos, calibraciones, cambios de umbral, un resumen por minuto (nivel medio, máximo, segundos activo y kB recibidos), cuándo la pantalla se oculta o se bloquea y los *huecos* en los que el móvil dejó de ejecutar la página (señal de que iOS la pausó). Guarda un máximo de 6.000 eventos (unos 4 días).
- **Anotar:** escribe notas como "batería 80 %" o "bloqueé el móvil" para cruzarlas con el registro; iOS no deja leer la batería desde una web.
- **Exportar:** genera un archivo `.csv`. En iPhone se abre la hoja de compartir (Guardar en Archivos, AirDrop, Mail…). Si no, se descarga o se copia.
- **Quitarlo más adelante:** en `index.html`, cambia `var TEST_LOG=true;` por `false` y el registro desaparece.

## Lista de pruebas en iPhone (lo que hay que validar)

- [ ] Emparejan con el PIN en la misma WiFi.
- [ ] Cierra y abre la página de padres: ¿se reconecta sola sin PIN?
- [ ] Cierra y abre la del bebé (pulsa *Activar micrófono*): ¿se reconectan solos?
- [ ] Apaga y enciende el WiFi del bebé: ¿cuánto tarda en volver?
- [ ] *Emparejar con otro móvil*: ¿pide PIN de nuevo en los dos?
- [ ] Móvil del bebé enchufado: ¿el audio llega con claridad a 10 m y con una pared?
- [ ] En el móvil de padres, *Ajustes → Calibrar* (10 s con el cuarto en silencio) y comprueba que el llanto suave sí pasa. Afina a mano con el control si hace falta.
- [ ] Móvil de padres con la pantalla bloqueada: ¿sigue sonando? (Safari puede cortar el audio en segundo plano; anota a los cuántos segundos.)
- [ ] Móvil del bebé con la pantalla bloqueada: ¿sigue escuchando? En las pruebas **sí** siguió enviando; por eso la pantalla se bloquea sola, y hay un interruptor *Mantener la pantalla encendida* en Ajustes si hiciera falta.
- [ ] Apaga el WiFi del bebé: ¿salta la alarma en padres en menos de 10 s?
- [ ] Consumo de batería de padres tras 1 hora.

## Limitaciones conocidas

- Usa WiFi (WebRTC), no Bluetooth: Safari en iPhone no admite Bluetooth web.
- Los iPhone pueden pausar Safari en segundo plano. Si la prueba lo confirma, el siguiente paso sería una app nativa.
- El volumen del móvil de padres se sube con los botones del iPhone.
- Para encontrarse (emparejar y reconectar) hace falta internet por WiFi. Con la conexión ya hecha, el audio sigue aunque se caiga internet.
- No es un producto sanitario: no sustituye la supervisión de un adulto.
