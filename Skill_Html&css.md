# Notas de clase – HTML y CSS

## ¿Qué es HTML?

**HTML** significa *HyperText Markup Language* (Lenguaje de Marcado de Hipertexto). Es el lenguaje con el que se construyen las páginas web, definiendo su estructura básica. Fue creado en 1990 con el objetivo de representar información estructurada en la web.

El HTML que trabajamos actualmente es **HTML5**, una versión más moderna, semántica y compatible con los navegadores actuales.

---

## Pasos para comenzar un proyecto

1. Crear una carpeta para el proyecto.
2. Abrir una terminal.
3. Ubicarse dentro de la carpeta creada:
   ```bash
   cd NombreDeLaCarpeta
   ```
4. Ejecutar:
   ```bash
   code .
   ```
   Esto abre la carpeta directamente en Visual Studio Code.
5. Crear un perfil en VS Code (si aplica).
6. Instalar las siguientes extensiones recomendadas:
   - `Live Server`: recarga automática del navegador.
   - `Git Graph`: visualiza ramas de git.
   - `Lorem Ipsum`: genera texto de prueba.
7. Crear el primer archivo:
   ```bash
   index.html
   ```

---

## Código base en HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Mi primera página</title>
  </head>
  <body>
    <h1>Hola Mundo</h1>
    <p>Este es mi primer párrafo.</p>
  </body>
</html>
```

---

## Explicación de etiquetas

| Etiqueta                  | Función                                                        |
|---------------------------|----------------------------------------------------------------|
| `<!DOCTYPE html>`         | Declara que el documento es HTML5                              |
| `<html>`                  | Indica que el contenido es un documento HTML                   |
| `<head>`                  | Sección de metadatos, configuraciones, título, enlaces, etc.   |
| `<meta charset="UTF-8">`  | Permite usar caracteres especiales como tildes y ñ              |
| `<title>`                 | Nombre de la pestaña del navegador                             |
| `<body>`                  | Contiene el contenido visible de la página web                 |

---

## Tipos de listas

### Lista ordenada

```html
<ol>
  <li>Elemento 1</li>
  <li>Elemento 2</li>
</ol>
```

### Lista desordenada

```html
<ul>
  <li>Elemento A</li>
  <li>Elemento B</li>
</ul>
```

---

## Atajo útil en Linux

- Para duplicar una línea en VS Code:
  ```
  Ctrl + Shift + Alt + 12
  
  ```

---


# CSS – Hojas de Estilo en Cascada

## ¿Qué es CSS?

**CSS** significa *Cascading Style Sheets* (Hojas de Estilo en Cascada). Es un lenguaje utilizado para describir la presentación de un documento HTML. Mientras que HTML estructura el contenido, CSS lo embellece, definiendo cómo se verá visualmente.

CSS permite aplicar estilos como colores, tipografía, márgenes, posiciones, tamaños, animaciones, entre otros aspectos visuales.

---

## ¿Por qué se llama "en cascada"?

Se llama "en cascada" porque los estilos se aplican con un sistema de prioridades. Por ejemplo:

1. Estilos definidos por el navegador (por defecto).
2. Estilos definidos por el usuario.
3. Estilos del desarrollador (los que escribimos en nuestros archivos CSS).

Cuando hay conflictos, CSS decide qué estilo aplicar según la **especificidad** y el **orden**.

---

## Formas de aplicar CSS

### 1. CSS en línea (inline)

Dentro de la misma etiqueta HTML:

```html
<p style="color: red;">Este texto es rojo.</p>
```

### 2. CSS interno (en el `<head>`)

```html
<head>
  <style>
    h1 {
      color: blue;
    }
  </style>
</head>
```

### 3. CSS externo (archivo aparte)

Se recomienda usar esta forma por organización y mantenimiento:

**Archivo `style.css`:**

```css
body {
  background-color: #f5f5f5;
  font-family: Arial, sans-serif;
}
```

**En el HTML:**

```html
<head>
  <link rel="stylesheet" href="style.css">
</head>
```

---

## Selectores en CSS

- **Universal:** `* { }` – Aplica a todos los elementos.
- **Etiqueta:** `p { }` – Aplica a todos los párrafos.
- **Clase:** `.clase { }` – Se usa para un grupo de elementos.
- **ID:** `#id { }` – Aplica a un único elemento.
- **Combinadores:** `div p { }`, `ul > li { }`, etc.

---

## Propiedades comunes

| Propiedad       | Función                              |
|----------------|---------------------------------------|
| `color`         | Color del texto                      |
| `background`    | Fondo del elemento                   |
| `font-size`     | Tamaño de la fuente                  |
| `margin`        | Margen exterior                      |
| `padding`       | Espaciado interno                    |
| `border`        | Borde del elemento                   |
| `width` / `height` | Ancho y alto                     |
| `display`       | Comportamiento del contenedor        |
| `position`      | Posicionamiento                      |
| `text-align`    | Alineación del texto                 |

---

## Unidades en CSS

- Relativas: `em`, `rem`, `%`
- Absolutas: `px`, `cm`, `mm`, `in`

---

## Colores

- Nombre: `red`, `blue`, `black`
- HEX: `#ff0000`
- RGB: `rgb(255, 0, 0)`
- RGBA: `rgba(255, 0, 0, 0.5)` (con opacidad)

---

## Comentarios en CSS

```css
/* Esto es un comentario */
```

---

## Buenas prácticas

- Usar CSS externo para mantener HTML limpio.
- Nombrar clases e IDs de forma descriptiva.
- Evitar usar `!important` a menos que sea estrictamente necesario.
- Organizar reglas por secciones (estructura, layout, componentes, etc).

---


### Documentación oficial recomendada

- [Guía CSS de Mozilla MDN](https://developer.mozilla.org/es/docs/Web/CSS)
