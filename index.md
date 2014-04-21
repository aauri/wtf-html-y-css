---
layout: default
---

### Contenido

- [Declara un doctype](#doctype)
- [Matemática del modelo de caja (*Box Model*)](#box-model-math)
- [Unidades `rem` y Safari móvil](#rems-mobile-safari)
- [Flotantes "*Floats*" primero](#floats-first)
- [*Floats* y la propiedad `clear`](#floats-clearing)
- [*Floats* y la altura calculada](#floats-computed-height)
- [*Floats* son a nivel de bloque (*block level*)](#floats-block-level)
- [Márgenes verticales a veces se cierran (*collapse*)](#vertical-margins-collapse)
- [Composición visual de filas de las tablas](#styling-table-rows)
- [Firefox y los botones `<input>` ](#buttons-firefox)
- [Firefox y los bordes internos de botones](#buttons-firefox-outline)
- [Asigne siempre un `type` a `<button>`](#buttons-type)
- [Límite de selectores en Internet Explorer](#ie-selector-limit)
- [Posicionamiento `position` aclarado](#position-explained)
- [Posicionamiento `position` y anchura `width`](#position-width)
- [Posicionamiento `fixed` con `transform`](#position-transforms)


<a name="doctype"></a>
### Declara un doctype
Incluye siempre un doctype. Les recomiendo el simple doctype de HTML5:

```html
<!DOCTYPE html>
```

[Omisión de el doctype puede causar problemas](http://quirks.spec.whatwg.org) con malformación de tablas, formularios `<input>`, y más problemas generando la página.


<a name="box-model-math"></a>
### Matemática del modelo de caja (*Box Model*)
Los elementos que tienen el ancho `width` especificado se alargan cuando tienen relleno `padding` y/o un borde con `border-width`. Para evitar estos problemas usa el ya común [*reset* `box-sizing: border-box;`](http://www.paulirish.com/2012/box-sizing-border-box-ftw/).


<a name="rems-mobile-safari"></a>
### Unidades `rem` y Safari móvil
Mientras que Safari móvil permite el uso de `rems` en todos los valores de la propiedad, se vuelve loco cuando se utilizan las `rems` en `media queries` y el texto de la página se vuelve infinitamente intermitente entre varios tamaños.

Por ahora, mejor usar las unidades `em` en vez de las unidades `rem`.

```css
html {
  font-size: 16px;
}

/* Causa problemas en Safari móvil */
@media (min-width: 40rem) {
  html {
    font-size: 20px;
  }
}

/* Funciona perfectamente bien en Safari móvil */
@media (min-width: 40em) {
  html {
    font-size: 20px;
  }
}
```

**Ayuda!** *Si tienes un link para reportar bugs a Apple o WebKit, me encantaría incluirlo aquí. Este problema sólo ocurre en la versión de Safari para dispositivos móviles y no en la versión de escritorio, por lo tanto, no se bien dónde reportar este error*.


<a name="floats-first"></a>
### Flotantes "*Floats*" primero
Los elementos flotantes `float` siempre deben venir primero en el orden del documento. Esto se debe a que los elementos `float` necesitan algo a que envolverse alrededor, si no, aparecen con defectos en la calculación de la posición. 
 

```html
<div class="parent">
  <div class="float">Float</div>
  <div class="content">
    <!-- … -->
  </div>
</div>
```


<a name="floats-clearing"></a>
### *Floats* y la propiedad `clear`
Si utilizas `float`, es probablemente necesario emplear la propiedad `clear` en el elemento subsecuente a uno `float`, para evitar que el elemento flote hacia arriba. Para usar la propiedad `clear`, puedes usar uno de estos ejemplos.

Aquí está el [micro clearfix](http://nicolasgallagher.com/micro-clearfix-hack/) que usa `clear` con una clase separada.

```css
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}
```

Alternativamente, especifica `overflow`, con `auto` o `hidden` en el elemento principal.

```css
.parent {
  overflow: auto; /* clearfix */
}
.other-parent {
  overflow: hidden; /* clearfix */
}
```

Tenga en cuenta que `overflow` puede causar otros defectos secundarios indeseables, generalmente alrededor de elementos posicionados dentro del elemento padre.

**Pro-Tip!** Hazte un favor a ti mismo y a tus colegas incluyendo el comentario `/* clearfix */` al utilizar la propiedad `clear` para `floats` ya que la propiedad se puede usar por otras razones.


<a name="floats-computed-height"></a>
### *Floats* y la altura calculada
Un elemento padre que solo tiene contenido `float` tendrá la altura calcuda `height: 0;`. Usa un *clearfix* en el elementro padre para obligar al navegador a calcular una altura.


<a name="floats-block-level"></a>
### *Floats* son a nivel de bloque (*block level*)
Elementos `float` son automaticamente a nivel de bloque (`display: block;`). No es necesario especificar `display` ya que sera ignorada por el navegador a menos que tenga el valor `none`.

```css
.element {
  float: left;
  display: block; /* No es necesario */
}
```

***Fun fact:*** *Hace años, tuvimos que especificar `display: inline;` para hacer que la propiedad `float` funcione correctamente en IE6 y evitar el [bug de margen duplicado](http://www.positioniseverything.net/explorer/doubled-margin.html). Sin embargo, esos días han quedado atrás.*


<a name="vertical-margins-collapse"></a>
### Márgenes verticalmente adyacentes se cierran (*collapse*)
Los márgenes superiores e inferiores de los elementos adyacentes (uno tras otro) se cierran (*collapse*) en varias situaciones, pero nunca en elementos posicionados `float` o `absolute`. Lee [este artículo MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/margin_collapsing) o la especificación CSS2 de márgenes cerrados en [Español](http://www.sidar.org/recur/desdi/traduc/es/css/box.html#collapsing-margins) o [Inglés](http://www.w3.org/TR/CSS2/box.html#collapsing-margins) para averiguar más.

Los márgenes horizontales *nunca se cierran*.


<a name="styling-table-rows"></a>
### Composición visual de filas de las tablas
Las filas de una tabla `<tr>` no pueden tener bordes a menos que uses el valor `border-collapse: collapse;` en el `<table>` padre. 
Por otra parte, si el `<tr>` y sus hijos `<td>` o `<th>` tienen el *mismo* `border-width`, las propiedades de los bordes de las líneas `<tr>` son ignoradas. [Puedes ver un ejemplo en este JS Bin](http://jsbin.com/yabek/2/)


<a name="buttons-firefox"></a>
### Firefox y los botones `<input>`
Por razones desconocidas, Firefox aplica un `line-height` a los botones de envío y `<input>` que no se puede modificar con CSS. A este punto, tienes dos alternativas para lidiar con esto:

1. Utiliza sólo los elementos `<button>`
2. No uses nunca `line-height` en tus botones.

Si usas la primera alternativa (y recomiendo ésta de todos modos porque los <button> son genialese) esto es lo que necesitas saber:

```html
<!-- No es tan bueno -->
<input type="submit" value="Enviar">
<input type="button" value="Cancelar">

<!-- Súper bien en todas partes -->
<button type="submit">Enviar</button>
<button type="button">Cancelar</button>
```

Si prefieres usar la segunda opción, simplemente excluye `line-height` y en cambio usa **sólo** `padding` para alinear el texto del botón. [Mira este ejemplo en JS Bin](http://jsbin.com/yabek/4/) en Firefox per ver el problema y la solución.

**¡Buenas noticias!** *Parece que a lo mejor habrá [una solución para esto](https://bugzilla.mozilla.org/show_bug.cgi?id=697451#c43) en Firefox 30. Estas son buenas noticias para nosotros en el futuro, pero ten en cuenta que no cambia los problemas en las versiones anteriores.*


<a name="buttons-firefox-outline"></a>
### Firefox y los bordes internos de botones

Firefox [añade bordes internos](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button#Notes) a los botones `<input>` y `<button>` en la propiedad `:focus`. Al parecer, es para la accesibilidad, pero su ubicación parece bastante extraña. Usa esta solución en CSS para ignorar estos bordes:


```css
input::-moz-focus-inner,
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
```

Puedes ver esta solución en acción en el mismo [ejemplo en JS Bin](http://jsbin.com/yabek/4/) mencionado en la sección anterior.

**Pro-Tip!** *No olvides incluir un estado `focus` para links y elementos `<button>` y `<input>`. Proveer utilidades para la accesibilidad es fundamental, tanto para los usuarios avanzados que usan pestañas para navegar contenido o los usuarios con problemas de vista.*


<a name="buttons-type"></a>
### Asigne siempre un `type` a `<button>`
El valor inicial es `submit`, es decir, cualquier botón en un formulario puede enviar el formulario. Es mejor usar `type="button"` para todo lo que no envía el formulario, y usar `type="submit"` para todos los botones para enviar.

```html
<button type="submit">Enviar</button>
<button type="button">Cancelar</button>
```

Para todas las acciones que requieren un `<button>` y no son parte de un formulario, usa `type="button"`.

```html
<button class="dismiss" type="button">x</button>
```

**Fun fact:** *Al parecer, IE7 no soporta adecuadamente el atributo `value` en elementos `<button>`. En lugar de leer el contenido del atributo, lee el "innerHTML" (el contenido entre la apertura y cierre de la etiqueta <button>). Sin embargo, no me preocupo mucho de esto por dos razones: el uso de IE7 está en baja, y es bastante raro el uso conjunto de un valor y "innerHTML" en `<button>`*


<a name="ie-selector-limit"></a>
### Límite de selectores en Internet Explorer
Internet Explorer 9 y versiones anteriores tienen un máximo de 4096 selectores por hoja de estilo. También tienen un límite de 31 hojas de estilo y `<style></style>` incluidos juntos por página. Cualquier cosa más después de este límite es omitido por el navegador. Te toca escoger entre dividir tu CSS o refactorización. Yo sugeriría el último. 

Por si acaso, así es como los navegadores cuentan selectores:

```css
/* Un selector */
.element { }

/* Más dos selectores */
.element,
.other-element { }

/* Más otros tres selectores */
input[type="text"],
.form-control,
.form-group > input { }
```


<a name="position-explained"></a>
### Posicionamiento `position` aclarado
Elementos `position: fixed;` son colocados en relación a la ventanilla *"viewport"* del navegador. Elementos `position: absolute;` se posicionan en relación a su bloque padre  más cercano que tenga una posición aparte a `static` (como por ejemplo, `relative`, `absolute`, o `fixed`).


<a name="position-width"></a>
### Posicionamiento `position` y anchura `width`
No asignes `width: 100%;` a un elemento con `position: [absolute|fixed];`, `left`, y `right`. El uso de `width: 100%;` es igual al uso combinado de `left: 0;` y `right: 0;`. Usa uno o el otro, pero no ambos.


<a name="position-transforms"></a>
### Posicionamiento `fixed` con `transform`
Navegadores interrumpen `position: fixed;` cuando el elemento padre tiene la propiedad `transform`. El uso de `transform` crea un bloque de contenido nuevo, y esto efectivamente obliga al padre a posicionarse `position: relative;` y el elemento `fixed` a comportarse como `position: absolute;`.

[Aquí esta un ejemplo](http://jsbin.com/yabek/1/) y puedes tambien leer el [post de Eric Meyers sobre este asunto](http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/).
