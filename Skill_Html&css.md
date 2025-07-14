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

## Introducción a CSS

**CSS** (Cascading Style Sheets o Hojas de Estilo en Cascada) es el lenguaje que usamos para aplicar estilos visuales a los documentos HTML.

### ¿Qué podemos hacer con CSS?

- Cambiar colores, tamaños, fuentes y márgenes.
- Organizar el contenido en columnas o filas.
- Aplicar animaciones y transiciones.
- Hacer que el sitio se vea bien en celulares y computadores.

---

### Documentación oficial recomendada

- [Guía CSS de Mozilla MDN](https://developer.mozilla.org/es/docs/Web/CSS)
