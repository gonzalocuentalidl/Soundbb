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

## Cómo emparejar (una vez cada vez que se abre)

Los dos móviles deben estar en la misma WiFi.

1. **Móvil del bebé:** pestaña *Móvil del bebé* → **Activar micrófono** (acepta el permiso) → **Crear código**.
2. Pulsa **Compartir** y envía el código por WhatsApp, iMessage o AirDrop al móvil de padres.
3. **Móvil de padres:** pestaña *Móvil de padres* → pega el código → **Conectar** (acepta el permiso del micrófono; solo sirve para que Safari use la red local, no graba nada).
4. Copia el código de respuesta y envíalo de vuelta al móvil del bebé.
5. En el móvil del bebé pégalo en *Código de respuesta* → **Conectar**.
6. Al conectar, las tarjetas de emparejamiento desaparecen y queda lo esencial.

Si se corta la conexión hay que repetir el emparejamiento (no hay servidor que lo recuerde).

## Lista de pruebas en iPhone (lo que hay que validar)

- [ ] Emparejan en la misma WiFi sin internet en el router (solo red local).
- [ ] Móvil del bebé enchufado, pantalla encendida: ¿el audio llega con claridad a 10 m y con una pared?
- [ ] Ajusta la **sensibilidad** (Ajustes) hasta que el ruido de fondo no dispare el envío.
- [ ] Móvil de padres con la pantalla bloqueada: ¿sigue sonando? (Safari puede cortar el audio en segundo plano; anota a los cuántos segundos.)
- [ ] Móvil del bebé con la pantalla bloqueada: ¿sigue escuchando? Lo normal es que **no**; por eso la página mantiene la pantalla encendida.
- [ ] Apaga el WiFi del bebé: ¿salta la alarma en padres en menos de 10 s?
- [ ] Consumo de batería de padres tras 1 hora.

## Limitaciones conocidas

- Usa WiFi (WebRTC), no Bluetooth: Safari en iPhone no admite Bluetooth web.
- Los iPhone pueden pausar Safari en segundo plano. Si la prueba lo confirma, el siguiente paso sería una app nativa.
- El volumen se controla dentro de la página; los botones del iPhone también funcionan.
- No es un producto sanitario: no sustituye la supervisión de un adulto.
