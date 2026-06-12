# Guía para agentes de código — Primera Comunión Emely Rafaela

Este documento resume la estructura, tecnología y convenciones del proyecto para que un agente de código pueda entenderlo y modificarlo sin suposiciones.

## Resumen del proyecto

- **Nombre:** Primera Comunión — Emely Rafaela Hurtado Barahona
- **Tipo:** Sitio web estático de una sola página (invitación personal).
- **Idioma del contenido:** Español.
- **Propósito:** Compartir la información del evento (fecha, hora, lugar, confirmación, fotos) y ofrecer una experiencia visual con animaciones suaves.
- **Estado actual:** Funcional y listo para publicarse en cualquier hosting estático.

## Tecnología y arquitectura

- **Stack:** HTML5 + CSS3 + JavaScript vanilla. No usa frameworks ni bibliotecas externas.
- **Sin dependencias de paquetes:** No existe `package.json`, `pyproject.toml`, `Cargo.toml`, `Gemfile`, `composer.json` ni similares.
- **Sin proceso de build:** No requiere compilación, empaquetado ni pasos de build. El navegador ejecuta directamente `index.html`.
- **Fuentes:** Se cargan desde Google Fonts vía CDN (`https://fonts.googleapis.com`):
  - `Cormorant Garamond`
  - `Great Vibes`
  - `Montserrat`
- **Recursos externos vinculados:**
  - Ubicación de la iglesia: Google Maps (`maps.app.goo.gl/Zs97oX5iaxL6gcPk9`)
  - Confirmación: WhatsApp (`wa.me/593989664303`)
  - Compartir fotos: Google Drive
- **Licencia:** Apache License 2.0 (ver `LICENSE`).

## Estructura de archivos

```
.
├── index.html       # Página única con todo: estructura, estilos y scripts
├── FOTOS/           # Fotografías usadas en la invitación
│   ├── 1.jpeg
│   ├── 2.jpeg
│   ├── 3.jpeg
│   ├── 4.jpeg
│   └── 5.jpeg
├── MUSICA/          # Música instrumental de fondo
│   └── fondo.mp3    # Pista descargada de Unminus (CC0)
├── README.md        # Documentación orientada a humanos
├── LICENSE          # Apache 2.0
├── .gitattributes   # Normalización de finales de línea: * text=auto
└── .git/            # Repositorio Git
```

### Notas sobre `index.html`

- Tiene aproximadamente 1050 líneas.
- Incluye `<style>` embebido para todos los estilos y `<script>` embebido para la lógica.
- No hay archivos `.css` ni `.js` separados.
- Secciones principales:
  1. Overlay de bienvenida (`#bienvenida`) con botón "Abrir invitación".
  2. Hero con foto principal, nombre y frase.
  3. Mensaje de bienvenida.
  4. Cuenta regresiva hasta el evento.
  5. Galería de fotos.
  6. Detalles del evento (fecha, hora, ceremonia, recepción).
  7. Itinerario.
  8. Confirmación por WhatsApp.
  9. Compartir fotos en Google Drive.
  10. Footer.
- Botones flotantes fijos: volver arriba, mapa, WhatsApp, subir fotos a Drive y play/pause de música.
- Animaciones: partículas doradas, aparición al hacer scroll (`IntersectionObserver`), flotación del marco central y transiciones CSS.

## Comandos de build y ejecución

No hay comandos de build. Para probar o visualizar:

1. **Abrir localmente:**
   ```bash
   # En Windows, desde PowerShell en la raíz del proyecto
   start index.html
   ```
   O simplemente hacer doble clic en `index.html`.

2. **Servir localmente (opcional):**
   ```bash
   python -m http.server 8000
   # Luego abrir http://localhost:8000
   ```

3. **Publicar:** subir todo el contenido de la carpeta (incluyendo `FOTOS/`) a GitHub Pages, Netlify, Vercel, Cloudflare Pages o cualquier hosting estático.

## Convenciones de estilo y modificaciones

- **Idioma:** Todo el contenido visible está en español. Mantener el español para cualquier texto nuevo.
- **CSS:** Escrito en un único bloque `<style>` dentro de `<head>`. Usa variables CSS en `:root` para la paleta de colores dorados y beige. Respetar esa paleta para mantener la coherencia visual.
- **JavaScript:** Escrito en un único bloque `<script>` antes de cerrar `</body>`. No usa módulos ES ni frameworks.
- **Imágenes:** Se referencian con rutas relativas como `FOTOS/4.jpeg`. Al cargar la página, JavaScript les agrega automáticamente el parámetro `?v=CACHE_VERSION`.
- **Música de fondo:** Se reproduce automáticamente al abrir la invitación (tras la interacción del usuario) mediante el elemento `<audio id="musica-fondo" loop>`. El volumen está fijado a 0.25 y se incluye un botón flotante para pausar/reanudar. La pista actual es "Autumn Allure" de Wowa, obtenida de Unminus bajo licencia CC0 (dominio público). Se referencia como `MUSICA/fondo.mp3` y JavaScript le agrega `?v=CACHE_VERSION` al cargar.
- **Versionado de recursos:** La constante `CACHE_VERSION` en el bloque `<script>` de `index.html` controla la versión de caché de imágenes y audio. Cuando se reemplace cualquier foto o el audio, incrementar `CACHE_VERSION` (por ejemplo, `'1'` → `'2'`). El `<meta property="og:image">` debe actualizarse manualmente al mismo número porque los crawlers de WhatsApp/Facebook no ejecutan JavaScript.
- **Fecha objetivo de la cuenta regresiva:**
  ```javascript
  const fechaEvento = new Date('2026-06-20T10:00:00-05:00').getTime();
  ```
  La zona horaria está fijada a UTC-5 (Ecuador). Si cambia la fecha/hora o zona del evento, modificar esta línea.
- **Responsividad:** El diseño está optimizado para móviles y se adapta a tablets/escritorio mediante media queries en `768px` y `480px`.

## Pruebas

- No hay suite de pruebas automatizadas.
- **Pruebas manuales recomendadas:**
  1. Abrir `index.html` en Chrome, Firefox, Safari y Edge.
  2. Verificar el modo responsive (p. ej., 375px de ancho) y en escritorio.
  3. Confirmar que el botón "Abrir invitación" oculta el overlay y habilita el scroll.
  4. Verificar que la cuenta regresiva disminuye cada segundo.
  5. Comprobar que los enlaces de WhatsApp, Maps y Drive abran la URL correcta.
  6. Asegurar que las imágenes de `FOTOS/` carguen sin errores 404.
  7. Confirmar que la música inicia al hacer clic en "Abrir invitación" y que el botón flotante la pausa/reanuda.

## Consideraciones de seguridad

- Todo el sitio es estático; no hay backend, base de datos, autenticación ni secretos.
- Los enlaces a servicios externos (WhatsApp, Google Maps, Google Drive) usan URLs completas y se abren en una nueva pestaña (`target="_blank"`).
- No se recolectan datos personales ni se usan cookies.
- Como buena práctica, si se agregan enlaces externos nuevos, mantener `rel="noopener noreferrer"` junto con `target="_blank"` para evitar vulnerabilidades de tabnabbing.

## Despliegue

1. Asegurarse de que `index.html` esté en la raíz del sitio publicado.
2. Incluir la carpeta `FOTOS/` con las imágenes en la misma ruta relativa (`FOTOS/`).
3. Opcional: configurar el dominio personalizado si el hosting lo permite.
4. Verificar la imagen de Open Graph (`<meta property="og:image" content="FOTOS/4.jpeg?v=3">`) para que WhatsApp/Facebook la muestren correctamente; algunas plataformas requieren una URL absoluta.

## Información del evento (resumen)

- **Celebrada:** Emely Rafaela Hurtado Barahona
- **Fecha:** Sábado 20 de junio de 2026
- **Hora:** 10:00 AM (UTC-5)
- **Ceremonia:** Iglesia (enlace a Google Maps en `index.html`)
- **Recepción:** En casa
- **Confirmaciones:** WhatsApp +593 98 966 4303 (hasta el jueves 18 de junio de 2026). Mensaje predefinido: "Te confirmo mi asistencia. Personas: _____".
- **Compartir fotos:** Carpeta de Google Drive enlazada en la página

---

Última actualización: 2026-06-12
