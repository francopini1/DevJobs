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

# CSS -Explicación de algúnas propiedades

# Uso de imagen de fondo en un "hero" — resumen detallado

Breve: hay dos enfoques principales para que una imagen ocupe el fondo de la sección `.hero` y que el formulario (input) quede por encima de ella hasta una línea concreta:

- Recomendado cuando la imagen es decorativa: usar `background-*` en CSS sobre el contenedor `.hero`.
- Alternativa cuando la imagen es contenido semántico: mantener un `<img>` y posicionarlo con `position: absolute` + `object-fit: cover`.

A continuación está la explicación de las propiedades clave, cuándo usarlas, cómo se combinan y ejemplos prácticos.

---

## 1. Propiedades clave y qué hacen (explicado para razonar)

- `background-image: url(...)`
  - Define la imagen de fondo del elemento. Uso: cuando la imagen es decorativa (no información necesaria).
- `background-size`
  - `cover`: la imagen cubre todo el contenedor, se recorta si es necesario. Útil en hero.
  - `contain`: la imagen se muestra completa y puede dejar espacios vacíos.
- `background-position`
  - Controla el punto focal de la imagen (ej. `top center`, `center center`, `right 30%`).
- `background-repeat`
  - Normalmente `no-repeat` para imágenes hero.
- `background` (shorthand)

  - Permite combinar: `background: url(...) center/cover no-repeat;`

- `position`
  - `relative` en el contenedor: crea contexto para posicionados absolutos internos y para `z-index`.
  - `absolute` en la imagen o overlay: posiciona respecto al ancestro posicionado.
- `z-index`
  - Manejo de apilamiento. Solo tiene efecto si el elemento está posicionado (`position` distinto de `static`).
- `::before` / `::after` (pseudo-elementos)
  - Útiles para overlays (capas semitransparentes) sin añadir HTML. Se suelen posicionar `absolute` y cubrir con `inset: 0`.
- `pointer-events`
  - `none` en overlays evita bloquear clicks hacia el contenido superior.
- `padding`, `min-height`, `height`, `vh`
  - La imagen de fondo solo se verá si el contenedor tiene dimensión. Usa `min-height` o `padding-bottom` para que el hero abarque hasta el input.
- `object-fit` (para `<img>`)
  - `cover/contain` aplican a imágenes reales. Si mantienes `<img>` y la posicionas, `object-fit: cover` la hace comportarse como `background-size: cover`.
- `overflow: hidden`
  - Si la imagen absoluta sobresale y quieres recortarla dentro del hero.

---

## 2. Cómo razonar la solución (pasos mentales)

1. ¿La imagen aporta contenido informativo?
   - Sí → usa `<img>` con `alt`.
   - No (solo decoración) → usa `background-image` en CSS.
2. ¿Necesitas un overlay para legibilidad?
   - Sí → usa `::before` o `::after` en `.hero`.
3. ¿El formulario debe estar encima del fondo?
   - Poner el contenido con `position: relative` y `z-index` mayor que el overlay.
4. ¿El fondo debe alcanzar hasta el input?
   - Aumentar `padding-bottom` o `min-height` del `.hero` para cubrirlo.
5. ¿Responsivo?
   - Ajustar `background-position` y `min-height` con media queries.

---

## 3. Ejemplos prácticos (sin tocar HTML)

Ejemplo recomendado: imagen como background en `.hero` + overlay + formulario por encima.

```css
/* enfoque recomendado: background en el contenedor */
.hero {
  position: relative; /* contexto para overlay y stacking */
  background-image: url("./background.webp");
  background-size: cover; /* ocupa todo, recorta si es necesario */
  background-position: top center; /* controla el punto focal */
  background-repeat: no-repeat;
  padding: 3.5rem 1.25rem 4.5rem; /* espacio inferior para incluir input */
  min-height: 360px; /* asegura altura mínima para ver el fondo */
  display: flex;
  flex-direction: column;
  gap: 1rem;
  color: var(--text-primary);
}

/* overlay para mejorar contraste */
.hero::before {
  content: "";
  position: absolute;
  inset: 0; /* top:0; right:0; bottom:0; left:0 */
  background: linear-gradient(rgba(6, 24, 42, 0.55), rgba(6, 24, 42, 0.35));
  z-index: 0;
  pointer-events: none; /* no bloquea clicks */
}

/* contenido por encima del overlay */
.hero > * {
  position: relative;
  z-index: 1;
}

/* formulario visible encima del fondo */
.search-form {
  position: relative;
  z-index: 2;
  display: flex;
  gap: 0.5rem;
  background: rgba(30, 41, 59, 0.55);
  padding: 0.6rem;
  border-radius: 8px;
  max-width: 800px;
}
```

# Explicación a partir de la linea 130

Te responderé cada pregunta en detalle:

1. **Position relative y absolute**:

   - Sí, exactamente. Cuando quieres posicionar elementos de forma absoluta, necesitas un contenedor padre con `position: relative`
   - El elemento con `position: absolute` se posicionará en relación al ancestro más cercano que tenga `position: relative`
   - Si no hay un padre con `position: relative`, el elemento absolute se posicionará en relación al viewport

2. **Background-image y content**:

   - Sí, puedes usar cualquier imagen como background-image usando: `url("ruta/a/tu-imagen.jpg")`
   - El `content: ""` en el pseudoelemento `::before` está vacío porque:
     - Los pseudoelementos REQUIEREN la propiedad `content` para ser visibles
     - En este caso solo queremos crear un overlay, no añadir contenido real
     - Si quisieras añadir texto, podrías hacer `content: "Texto aquí"`

3. **Background propiedades**:

   - `no-repeat` se usa principalmente con backgrounds, sí. Evita que la imagen se repita si el contenedor es más grande que la imagen
   - `linear-gradient()` es una función que crea un degradado:
     ```css
     background: linear-gradient(rgba(6, 24, 42, 0.55), rgba(6, 24, 42, 0.35));
     ```
     - El primer color: `rgba(6, 24, 42, 0.55)` - azul oscuro con 55% de opacidad
     - El segundo color: `rgba(6, 24, 42, 0.35)` - el mismo azul con 35% de opacidad
     - Crea una transición suave entre estos colores

4. **Height propiedades**:

   - `min-height`: Establece la altura mínima que DEBE tener el elemento
     - Útil cuando quieres asegurar que algo tenga al menos cierta altura
     - El elemento puede crecer más allá de este valor
   - `max-height`: Establece la altura máxima que PUEDE tener el elemento
     - Útil para prevenir que elementos crezcan demasiado
     - El elemento nunca superará este valor

5. **Gap vs Margin**:

   - `gap` solo funciona en contenedores flex o grid
   - Solo afecta el espacio ENTRE elementos hermanos
   - `margin` afecta el espacio alrededor de un elemento individual
   - `gap` es más fácil de mantener en layouts flexbox/grid porque no necesitas preocuparte por márgenes que colapsan

6. **Inset**:

   - Es una propiedad abreviada que establece todas las posiciones a la vez
   - `inset: 0` es equivalente a:
     ```css
     top: 0;
     right: 0;
     bottom: 0;
     left: 0;
     ```

7. **Pointer-events**:

   - Controla cómo el elemento responde a eventos del mouse
   - `pointer-events: none` hace que el elemento sea "atravesable"
   - Útil para overlays donde quieres que los clics pasen al elemento debajo

8. **Z-index**:

   - Controla el orden de apilamiento de elementos
   - Mayor número = más arriba en la pila
   - Solo funciona en elementos posicionados (relative, absolute, fixed)
   - Por defecto todos los elementos tienen `z-index: auto` (equivalente a 0)

9. **Overlay**:
   - Es una capa semitransparente que se coloca encima de otro contenido
   - En tu caso, el overlay oscurece ligeramente la imagen de fondo para mejorar la legibilidad del texto
   - Se crea usando el pseudoelemento `::before` con un fondo semitransparente
   - Es común en diseño web para:
     - Mejorar el contraste del texto sobre imágenes
     - Crear efectos de hover
     - Mostrar modales o popups

El overlay en tu código específico está creado usando el pseudoelemento `::before` con un gradiente semitransparente, posicionado absolutamente para cubrir todo el hero, y con un `z-index` bajo para asegurarse de que el contenido principal aparezca por encima.

**Css-midutips**

# Css tiene variables, en la cual se denomina Custom-propierties
