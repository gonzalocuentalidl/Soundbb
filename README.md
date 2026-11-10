# Vigilabebés Local

Un solo archivo (`index.html`) que convierte dos móviles en un vigilabebés. Sin servidor, sin cuenta y sin datos móviles: el audio viaja directo entre los dos móviles por el WiFi de casa.

- **Móvil del bebé:** escucha y solo envía sonido cuando detecta ruido (con 5 s de margen para no cortar el llanto).
- **Móvil de padres:** reproduce el sonido, muestra el último sonido y suena una alarma si se pierde la señal.

## Publicarlo en GitHub Pages (5 minutos)

1. En github.com, crea un repositorio nuevo (por ejemplo `vigilabebes`), público.
2. Pulsa **Add file → Upload files** y sube `index.html` (y este README si quieres). Confirma con **Commit changes**.
3. Ve a **Settings → Pages**. En *Build and deployment* elige **Deploy from a branch**, rama `main`, carpeta `/ (root)` y guarda.
4. Espera 1-2 minutos. El enlace será `https://TU-USUARIO.github.io/vigilabebes/`.
5. Abre ese enlace en los dos iPhone con Safari. Opcional: Compartir → *Añadir a pantalla de inicio*; pero para vigilar de verdad es más fiable dejarlo en una pestaña de Safari (ver pruebas).

## Cómo emparejar (cada vez que se abre)

Los dos móviles deben estar en el mismo WiFi y con internet (solo para el PIN).

1. **Móvil del bebé:** pestaña *Móvil del bebé* → **Crear PIN** (acepta el permiso del micrófono). Aparece un PIN de 6 cifras.
2. **Móvil de padres:** pestaña *Móvil de padres* → escribe el PIN → **Conectar** (acepta el permiso del micrófono; solo sirve para que Safari use la red local, no graba nada).
3. En unos segundos conectan y las tarjetas de emparejamiento desaparecen.

Cómo funciona: el PIN solo sirve para que los dos móviles se encuentren a través de un servicio público gratuito (ntfy.sh). El audio va directo de móvil a móvil por tu WiFi y no pasa por internet. El PIN caduca a los 5 minutos y es aleatorio, así que nadie puede adivinarlo mientras no esté en pantalla. Si se corta la conexión hay que crear un PIN nuevo.

## Lista de pruebas en iPhone (lo que hay que validar)

- [ ] Emparejan con el PIN en la misma WiFi.
- [ ] Móvil del bebé enchufado, pantalla encendida: ¿el audio llega con claridad a 10 m y con una pared?
- [ ] Ajusta la **sensibilidad** (Ajustes) hasta que el ruido de fondo no dispare el envío.
- [ ] Móvil de padres con la pantalla bloqueada: ¿sigue sonando? (Safari puede cortar el audio en segundo plano; anota a los cuántos segundos.)
- [ ] Móvil del bebé con la pantalla bloqueada: ¿sigue escuchando? Lo normal es que **no**; por eso la página mantiene la pantalla encendida.
- [ ] Apaga el WiFi del bebé: ¿salta la alarma en padres en menos de 10 s?
- [ ] Consumo de batería de padres tras 1 hora.

## Limitaciones conocidas

- Usa WiFi (WebRTC), no Bluetooth: Safari en iPhone no admite Bluetooth web.
- Los iPhone pueden pausar Safari en segundo plano. Si la prueba lo confirma, el siguiente paso sería una app nativa.
- El volumen del móvil de padres se sube con los botones del iPhone.
- Para emparejar hace falta internet por WiFi (sin él, la conexión no se puede iniciar).
- No es un producto sanitario: no sustituye la supervisión de un adulto.
