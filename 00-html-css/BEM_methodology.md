# Metodología BEM (Block Element Modifier)

## ¿Qué es BEM?

BEM es una metodología de nombrado para clases CSS que ayuda a crear código más mantenible y reutilizable. El nombre BEM viene de:
- **B**lock (Bloque)
- **E**lement (Elemento)
- **M**odifier (Modificador)

## Estructura Básica

### 1. BLOCK (Bloque)
- Componente independiente y reutilizable
- Se nombra con una palabra descriptiva
- Usa minúsculas y guiones medios para palabras múltiples

```css
.header { }      /* El header principal */
.search-form { } /* El formulario de búsqueda */
.job-card { }    /* Una tarjeta de trabajo */
```

### 2. ELEMENT (Elemento)
- Parte de un bloque sin significado independiente
- Se conecta al bloque usando dos guiones bajos: `block__element`

```css
.header__logo { }           /* El logo dentro del header */
.search-form__input { }     /* El input dentro del formulario */
.job-card__title { }        /* El título dentro de la tarjeta */
```

### 3. MODIFIER (Modificador)
- Define una variación o estado
- Se conecta usando dos guiones: `block--modifier` o `block__element--modifier`

```css
.pagination__button--active { }    /* Botón de paginación activo */
.pagination__button--disabled { }  /* Botón de paginación deshabilitado */
```

## Ejemplos Prácticos

### Header con Navegación
```html
<header class="header">
    <div class="header__logo">
        <!-- Logo y nombre -->
    </div>
    <nav class="header__nav">
        <a class="header__nav-link">Empleos</a>
    </nav>
</header>
```

### Formulario de Búsqueda
```html
<form class="search-form">
    <div class="search-form__input-group">
        <svg class="search-form__icon">...</svg>
        <input class="search-form__input" type="search">
    </div>
</form>
```

### Tarjeta de Trabajo
```html
<article class="job-card">
    <h4 class="job-card__title">Ingeniero de Software</h4>
    <p class="job-card__meta">Tech Solutions Inc.</p>
    <button class="job-card__button">Aplicar</button>
</article>
```

## Beneficios de BEM

1. **Claridad**
   - Estructura visible en el nombre de la clase
   - Cambios predecibles y controlados

2. **Modularidad**
   - Bloques independientes y reutilizables
   - Portabilidad entre páginas

3. **Mantenibilidad**
   - Estilos predecibles
   - Relación clara entre HTML y CSS

## Reglas Importantes

### 1. No Anidar Más de Un Nivel
```css
/* BIEN */
.block__element { }

/* EVITAR */
.block__element__subelement { } /* Demasiado anidado */
```

### 2. Usar Modificadores Correctamente
```css
/* BIEN */
.job-card--featured { }
.job-card__button--primary { }

/* EVITAR */
.job-card-featured { } /* No sigue la convención BEM */
```

### 3. Nombres Descriptivos y Concisos
```css
/* BIEN */
.search-form__input { }

/* EVITAR */
.search-form__job-search-input-field { } /* Demasiado largo */
```

## Consejos Adicionales

1. Mantén los nombres en inglés para consistencia
2. Usa modificadores solo cuando sea necesario
3. No repitas el nombre del bloque en los elementos si no es necesario
4. Mantén la especificidad baja
5. Evita selectores anidados cuando sea posible

## Conclusión

La metodología BEM es una herramienta poderosa para mantener tu código CSS organizado y escalable. Siguiendo estas convenciones, tu código será más fácil de mantener y entender por todo el equipo.