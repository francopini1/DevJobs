- La etiqueta `<meta name="description" content="Aqui iria el contenido de cuando se realiza la busqueda, y sale la descripcion">`
- Tenemos dentro de la etiqueta `form` (el atributo `role`, que básicamente indica cual seria el rol, uno puede ser `search` = buscar)
- Lo más recomendado al día de hoy si se desea poner iconos es buscar Tabler Icons, que son en formato SVG  
  (url: https://tabler.io/icons)
- Está el metadato `robots` `content="index, follow"`: indica si la página debe indexarse y si debe seguir los enlaces.
- Hay dos etiquetas que no tienen un contexto semántico: `span` (agrupa contenido en línea) y `div` (agrupa contenido en bloque).
- El HTML semántico refiere a un contexto con sentido.

---

GitHub Copilot

## Breve resumen — diferencias clave entre `<video>` y `<iframe>`

- Propósito

  - `<video>`: Reproduce archivos de medios (`mp4`, `webm`, `ogg`) o streams directos. Es para contenido multimedia controlado por el navegador.
  - `<iframe>`: Inserta una página o recurso HTML externo (documento, reproductor embebido como YouTube). No es un reproductor nativo por sí mismo.

- Qué puedes pegar en cada uno

  - `<video>`: rutas a archivos/streams (`./video.mp4`, `https://cdn/.../stream.webm`). No puede cargar páginas (por ejemplo, una URL de YouTube).
  - `<iframe>`: cualquier URL que sirva HTML (por ejemplo `https://www.youtube.com/embed/ID`).

- Controles y accesibilidad

  - `<video>`: atributos nativos (`controls`, `autoplay`, `muted`, `loop`, `poster`), soporte de `<track>` para subtítulos.
  - `<iframe>`: no tiene controles de reproducción nativos — el contenido embebido debe exponerlos; usa `title`, `sandbox` y `allow` para accesibilidad y permisos.

- Formatos y compatibilidad

  - `<video>`: depende de códecs y formatos soportados por el navegador (`mp4`/H.264, `webm`/VP9, etc.).
  - `<iframe>`: el navegador solo renderiza la página remota; la compatibilidad depende del contenido embebido.

- Seguridad / aislamiento
  - `<video>`: carga directa del recurso (posibles problemas CORS para ciertas fuentes).
  - `<iframe>`: puede aislarse con `sandbox`, permite restringir permisos y separar contexto.

## Ejemplos

Video local (funciona si tienes el archivo):

```html
<video src="./videos/miVideo.mp4" controls poster="./images/poster.jpg">
  <track kind="captions" src="captions.vtt" srclang="es" label="Español" />
  Tu navegador no soporta el elemento <video>.</video>
</video>
```

YouTube embed (usa `<iframe>`, no `<video>`):

```html
<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/i9M7gdzlA7I?start=80"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen>
</iframe>
```

Conclusión rápida: si tienes un archivo de vídeo usa `<video>`; si quieres incrustar un reproductor externo (YouTube/Vimeo/página completa) usa `<iframe>`. Si quieres, te adapto el bloque de tu archivo para que el vídeo se reproduzca correctamente.

-CSS:
