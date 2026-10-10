# CSS

## Unidad 2 · Selectores

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de selectores. Los selectores definen a qué elementos se aplican las declaraciones y dominarlos es la diferencia entre CSS mantenible y CSS lleno de !important. Aquí gana el selector corto y semántico, no el más ingenioso. Pregunta inicial: ¿cuántos niveles de anidamiento aguantáis antes de perder el hilo?

---

## Objetivos de aprendizaje

<span class="fragment">1. Distinguir <mark>selectores simples y complejos</mark> y leerlos de derecha a izquierda</span>

<span class="fragment">2. Aplicar los <mark>cuatro combinadores</mark> sin confundir más con tilde</span>

<span class="fragment">3. Emplear pseudo-clases <mark>estructurales, de estado y funcionales</mark></span>

<span class="fragment">4. Generar contenido decorativo con <mark>pseudo-elementos</mark></span>

<span class="fragment">5. Calcular la especificidad incluidos <mark>:is(), :where() y :has()</mark></span>

Note:
Cinco objetivos, todos aplicables desde la primera práctica. El segundo y el quinto son los que más se examinan porque combinan memorización y cálculo. El cuarto conecta con accesibilidad: el contenido generado puede leerse en algunos lectores de pantalla.

---

## Por qué importan los selectores

<span class="fragment">Definen <mark>a qué elementos</mark> llega cada declaración del bloque</span>

<span class="fragment">Un selector corto y semántico evita llegar al <mark>!important</mark></span>

<span class="fragment">Selectores profundos encarecen el <mark>estilo incremental</mark> del motor</span>

<span class="fragment">Son la parte del CSS que más se <mark>reutiliza</mark> entre proyectos</span>

<span class="fragment">Los selectores se compilan una vez y se evalúan en cada cambio del DOM</span>

<span class="fragment">Un selector con <mark>clases</mark> se reutiliza en cualquier parte del proyecto</span>

Note:
El selector es la mitad de cualquier regla CSS, y suele ser la mitad que se escribe mal. Cuanto más profundo es el selector, más difícil es de sobreescribir y más trabajo le da al motor. Si aprendéis una sola cosa hoy, que sea escribir selectores cortos y con clases. Los selectores no solo afectan al tamaño del CSS: también a la facilidad con la que otra persona puede tocarlo.

---

## Taxonomía de un selector

```text
Selector complejo = [combinador] + selector(es) simple(s)
Simple = tipo | universal | clase | ID | atributo
        | pseudo-clase | pseudo-elemento
Ejemplo: nav > ul li a[href^="https"]:not(.externo)::after
```

<span class="fragment">Se lee de <mark>derecha a izquierda</mark>: lo último es lo que se busca</span>

<span class="fragment">Lo que queda a la izquierda son <mark>condiciones de contexto</mark></span>

<span class="fragment">La parte derecha es el <mark>objeto</mark> sobre el que se evalúa el resto</span>

Note:
Leer un selector de derecha a izquierda es el hábito que más rápido os va a hacer avanzar. El elemento de la derecha es el que se busca; a la izquierda solo hay ancestros, padres o hermanos que condicionan la búsqueda. Practicad con el ejemplo del bloque hasta que salga solo.

---

## Selectores simples · clase, ID y tipo

<span class="fragment"><mark>Tipo</mark>: h1, p, div; no distingue mayúsculas en HTML</span>

<span class="fragment"><mark>Clase</mark>: .alerta, el caballo de batalla del día a día</span>

<span class="fragment"><mark>ID</mark>: #inicio, único por documento y de alta especificidad</span>

<span class="fragment"><mark>Universal</mark>: asterisco, úsalo con cuidado en los resets</span>

<span class="fragment">Clases semánticas como .tarjeta__precio, no visuales como .caja-blanca</span>

Note:
La clase es la unidad de trabajo: reutilizable, semántica y de especificidad moderada. El ID debe ser único y dificulta el override, así que no sirve para maquetar componentes repetibles. El selector universal toca todos los nodos del árbol, evítalo en reglas que se apliquen a menudo.

---

## Selectores de atributo

| Sintaxis | Significado | Ejemplo |
|---|---|---|
| `[attr]` | Existe | `input[required]` |
| `[attr$="v"]` | Termina en | `a[href$=".pdf"]` |
| `[attr^="v"]` | Empieza por | `a[href^="https"]` |

<span class="fragment">También hay <code>^=</code>, <code>*=</code> y <code>~=</code>; <code>i</code> ignora mayúsculas</span>

<span class="fragment">Sirve para estado y tipo de recurso: PDF, externos</span>

<span class="fragment">Con <code>:has()</code> cubre casi cualquier caso sin clases</span>

Note:
Los selectores de atributo estilizan según lo que diga el propio HTML sin añadir una sola clase. El más útil en formularios combina atributos ARIA con pseudo-clases, por ejemplo el menú expandido. Recordad la triada: el acento de techo para empieza, el dólar para termina y el asterisco para contiene. Y el flag i os salva cuando el HTML viene de un CMS que no distingue mayúsculas.

---

## Combinadores

<span class="fragment">Espacio: <mark>descendiente</mark> en cualquier nivel, article p</span>

<span class="fragment"><code>&gt;</code>: <mark>hijo directo</mark>, ul &gt; li</span>

<span class="fragment"><code>+</code>: hermano siguiente <mark>inmediato</mark>, h2 + p</span>

<span class="fragment"><code>~</code>: hermano <mark>general</mark>, no hace falta que sea el siguiente</span>

<span class="fragment">Acotad la profundidad: <mark>máximo dos o tres niveles</mark></span>

Note:
Los combinadores son el pegamento que define el contexto de búsqueda. La confusión clásica es entre más y tilde: el primero exige inmediación y el segundo no. El descendiente además no pide parentesco directo: basta con que esté por debajo en el árbol del documento.

--

## Combinadores con lupa: ¿A quién afecta?

```html
<article>
  <h2>Título</h2>
  <p>Párrafo 1 (inmediato tras h2)</p>
  <div><p>Párrafo 2 (dentro de un div)</p></div>
  <p>Párrafo 3 (hermano posterior)</p>
</article>
```

<span class="fragment"><code>article p</code> (descendiente): selecciona los <strong>3 párrafos</strong> (cualquier profundidad)</span>

<span class="fragment"><code>article &gt; p</code> (hijo directo): selecciona párrafos 1 y 3 (excluye al párrafo 2 del <code>div</code>)</span>

<span class="fragment"><code>h2 + p</code> (hermano adyacente): <strong>solo el Párrafo 1</strong> (justo a continuación)</span>

<span class="fragment"><code>h2 ~ p</code> (hermanos generales): selecciona párrafos 1 y 3 (comparten padre)</span>

Note:
Regla para el examen:
1. El espacio no exige ser padre e hijo; llega hasta el infinito del árbol.
2. Los combinadores de hermanos (+ y ~) solo buscan hacia abajo en el documento, nunca hacia arriba.

---

## Pseudo-clases estructurales

<span class="fragment"><code>:root</code> es el elemento raíz: ahí viven las variables globales</span>

<span class="fragment"><code>:first-child</code>, <code>:last-child</code> y <code>:only-child</code></span>

<span class="fragment"><code>:nth-child(n)</code> cuenta a <mark>todos</mark> los hermanos</span>

<span class="fragment"><code>:nth-of-type(n)</code> cuenta solo los del <mark>mismo tipo</mark></span>

<span class="fragment"><code>:empty</code> marca los elementos sin hijos ni texto</span>

Note:
Una pseudo-clase describe un estado, una posición o una función del elemento y se escribe con un solo dos puntos. La diferencia entre nth-child y nth-of-type es la que más errores produce en los ejercicios. En el examen suelen preguntarla con un ejemplo concreto de lista.

---

## La fórmula An+B

```css
:nth-child(2n)      /* pares: 2, 4, 6 */
:nth-child(2n+1)    /* impares: 1, 3, 5 */
:nth-child(3n+1)    /* 1, 4, 7: primero de cada tres */
:nth-child(-n+3)    /* solo los tres primeros */
```

<span class="fragment"><code>n</code> es el paso y <code>B</code> el desplazamiento inicial</span>

<span class="fragment">También valen los alias <code>even</code> y <code>odd</code></span>

<span class="fragment">Truco visual para 3n+1: imaginar los hijos en <mark>filas de tres</mark></span>

Note:
La fórmula parece álgebra pero es visual: dibujad los hermanos y buscad el patrón. El menos n más tres es el truco clásico para quedarse con los tres primeros y aparece mucho en tablas con filas alternas. Si os falla un nth-child, revisad primero si queríais nth-of-type.

---

## Estados de la interfaz

<span class="fragment"><code>:hover</code> y <code>:active</code>: encima y mientras se mantiene pulsado</span>

<span class="fragment"><code>:focus</code> frente a <mark>:focus-visible</mark>, el foco accesible</span>

<span class="fragment"><code>:focus-within</code>: él o cualquier descendiente tiene foco</span>

<span class="fragment">Orden de escritura: base, hover, focus-visible, active, disabled</span>

<span class="fragment"><code>:target</code> marca el elemento del fragmento de la URL</span>

Note:
El focus-visible es la pseudo-clase estrella de la unidad: muestra el anillo solo cuando la navegación es por teclado y evita que se vea feo con el ratón. Nunca pongáis outline sin dejar una alternativa visible. El orden de los estados evita que una regla pise a otra sin querer.

---

## Estados de formularios

<span class="fragment"><code>:valid</code> e <code>:invalid</code> según la validación nativa del navegador</span>

<span class="fragment"><code>:user-valid</code> y <code>:user-invalid</code> solo tras tocar el campo</span>

<span class="fragment"><code>:required</code>, <code>:optional</code>, <code>:disabled</code></span>

<span class="fragment"><code>:checked</code> para los radio y checkbox marcados</span>

<span class="fragment">Pseudo-clases nativas antes que <mark>clases manuales de JavaScript</mark></span>

Note:
El navegador ya sabe si un campo es válido: CSS os deja pintarlo sin una sola línea de JavaScript. El user-invalid es más amable porque no pinta en rojo mientras el usuario todavía está escribiendo. Este contenido conecta directamente con los formularios del módulo de HTML.

---

## Pseudo-clases funcionales

<span class="fragment"><code>:is()</code> agrupa selectores y aporta la especificidad máxima</span>

<span class="fragment"><code>:where()</code> hace lo mismo pero aporta <mark>especificidad cero</mark></span>

<span class="fragment"><code>:not()</code> niega cualquier selector complejo</span>

<span class="fragment">Las tres evitan repetir la misma regla: principio <mark>DRY</mark></span>

<span class="fragment">Todas son <mark>forgiving</mark>: un argumento inválido no rompe la regla</span>

Note:
:is() y :where() son la misma idea con distinto peso en la especificidad. Elegid :where() para las reglas base que queráis que cualquiera pueda pisar después. El :not() admite selectores complejos desde Selectors Level 4, lo que amplía muchísimo su utilidad.

---

## :has(), el selector padre

```css
/* tarjeta con imagen: cambia el layout */
.card:has(> img) { grid-template-columns: 200px 1fr; }
/* campo con error marcado por el navegador */
.campo:has(input:user-invalid) { border-color: crimson; }
/* item de nav con el submenu abierto */
.nav-item:has([aria-expanded="true"]) { background: #eee; }
```

<span class="fragment">Selecciona un elemento si <mark>contiene</mark> algo que cumple la condición</span>

<span class="fragment">Admite combinadores, incluso <mark>hacia atrás</mark> con más y tilde</span>

<span class="fragment">Permite lógica condicional <mark>sin JavaScript</mark></span>

Note:
El :has() llevó años esperando en borrador y ya está en los tres grandes navegadores. Es lo que antes se resolvía con JavaScript o con trucos de hermanos. Si os preguntan qué selector permite estilizar al padre de algo, la respuesta es este.

---

## Pseudo-elementos

<span class="fragment">Representan <mark>partes</mark> del elemento y se escriben con dos puntos</span>

<span class="fragment"><code>::before</code> y <code>::after</code> generan cajas ficticias</span>

<span class="fragment">Sin <code>content</code> el pseudo-elemento <mark>no se genera</mark></span>

<span class="fragment">También hay <code>::first-line</code>, <code>::marker</code>, <code>::selection</code></span>

<span class="fragment">No aparecen en el DOM: cuidado con el <mark>contenido decorativo</mark></span>

Note:
El doble dos puntos es la marca del pseudo-elemento y el simple el de la pseudo-clase. En código nuevo siempre se escribe doble aunque los navegadores acepten uno por compatibilidad. Si el contenido es decorativo, marcadlo con aria-hidden para no confundir a los lectores de pantalla.

---

## Ejemplo · Iconos con ::before

```css
.icono::before {
  content: ""; /* obligatorio: cadena, contador o url */
  display: inline-block;
  width: 1em;
  height: 1em;
  background: currentColor;
  mask: url("https://dummyimage.com/800x600/ccc/000.png&text=icon.svg") center / contain no-repeat;
}
.cita::before { content: open-quote; }
```

<span class="fragment"><code>currentColor</code> hereda el color del texto y ahorra reglas</span>

<span class="fragment">Los contadores numeran pasos e índices sin tocar el HTML</span>

<span class="fragment">Un icono así <mark>no añade imágenes</mark> al documento</span>

Note:
Este patrón es el uso profesional clásico del ::before: iconos decorativos sin tocar el HTML. El currentColor hace que el icono se pinte con el color del texto, así que un solo cambio lo mueve todo a la vez. Es el ejemplo perfecto de separación de preocupaciones.

---

## Especificidad aplicada

```css
p            { color: red; }    /* 0-0-1 : un tipo */
.tarjeta     { color: blue; }   /* 0-1-0 : una clase */
#hero        { color: green; }  /* 1-0-0 : un ID */
div::before  { color: gray; }   /* 0-0-2 : tipo + pseudo-elemento */
```

<span class="fragment">Pseudo-clases y atributos cuentan como <mark>clase</mark></span>

<span class="fragment">Pseudo-elementos cuentan como <mark>tipo</mark></span>

<span class="fragment">Estrategia anti-guerra: <mark>clases</mark>, poca profundidad y capas</span>

Note:
La especificidad se calcula sobre el selector completo, incluidas pseudo-clases y pseudo-elementos. La estrategia profesional no es ganar la guerra de especificidad, sino no declararla: clases cortas, dos niveles como mucho y @layer para ordenar. Si os sobran !important, el problema es de arquitectura.

---

## Ejemplo práctico · Selectores modernos y relacionales (:has, :is, :not)

```css
/* 1. Combinador adyacente: primer párrafo tras titular */
h1 + p { font-style: italic; color: #555; }

/* 2. Pseudo-clase estructural: filas alternas en tabla */
.tabla-inventario tr:nth-child(even) { background-color: #f9f9f9; }

/* 3. Selector relacional :has() (selector de padre nativo) */
.tarjeta-producto:has(button:disabled) {
  opacity: 0.6;
  filter: grayscale(1);
}

/* 4. Pseudo-clases lógicas :is() y :not() */
:is(button, a.boton):not(:disabled) {
  cursor: pointer;
  transform: scale(1.02);
}
```

<span class="fragment"><code>:has()</code> evalúa los descendientes para aplicar estilos al elemento contenedor</span>

<span class="fragment"><code>:nth-child(even)</code> automatiza el diseño cebra sin clases manuales en el HTML</span>

<span class="fragment"><code>:is()</code> agrupa selectores tomando la especificidad del más pesado</span>

Note:
Guía didáctica de selectores avanzados:
1. h1 + p selecciona únicamente el párrafo inmediatamente adyacente tras el h1, ideal para entradillas periodísticas.
2. :has() resuelve la mayor demanda histórica de CSS: actuar como selector de padre. Si la tarjeta contiene un botón deshabilitado (sin stock), toda la tarjeta atenúa su opacidad sin necesidad de manipular clases con JavaScript.
3. :is(button, a.boton) evita duplicar reglas CSS para elementos con aspecto idéntico.
4. :not(:disabled) excluye estados inactivos asegurando interactividad limpia.

---

## Error común: nth-child y nth-of-type

⚠ `li:nth-child(2)` solo coincide si el segundo hijo **es** un li.

⚠ `li:nth-of-type(2)` es el segundo li aunque haya elementos entre medias.

⚠ Cambiad `:before` por `::before`: en código nuevo el doble es la norma.

⚠ `outline: none` sin alternativa deja al usuario **sin foco visible**.

Note:
Los dos primeros son el error más frecuente de la unidad y se detectan en cuanto se prueba en el navegador. Los dos últimos son de estilo y de accesibilidad: el tercero por especificación y el cuarto por criterio de evaluación. Los cuatro aparecen en ejercicios de convocatorias anteriores.

---

## Autoevaluación

¿Qué selecciona `form :nth-child(2n) input` y qué selecciona `form input:nth-child(2n)`?

--

El primero busca inputs dentro de un hijo par de form. El segundo pide que el propio input sea par, sin importar los hermanos que tenga antes.

Note:
La diferencia está en dónde cae el combinador descendiente y a qué elemento afecta la pseudo-clase. Esta pregunta separa a quien ha entendido la lectura de derecha a izquierda de quien solo ha memorizado. Contestadla señalando en el árbol los dos casos antes de avanzar.

---

## Claves para el examen

- **Combinadores**: espacio descendiente, `>` hijo, `+` inmediato, `~` general.
- **Pseudo-clase** = estado o posición con `:`; **pseudo-elemento** con `::`.
- `:nth-child()` cuenta **todos** los hermanos; `:nth-of-type()` solo su tipo.
- `:where()` aporta **especificidad 0**; `:is()` y `:has()` la de su argumento mayor.
- `:focus-visible` es el foco accesible; `outline: none` sin alternativa es error.
- Para no pelear: **clases**, poca profundidad y `:where()` combinado con `@layer`.

Note:
Seis claves para el repaso de la noche anterior al examen. La de focus-visible aparece en casi todas las convocatorias porque une especificidad con accesibilidad. La de combinadores es la que más puntos regala, porque basta con memorizar un ejemplo de lista.
