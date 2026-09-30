# CSS

## Unidad 5 · Flexbox

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Flexbox es el modelo de distribución en una dimensión: una fila o una columna. Sirve para repartir espacio entre items y alinearlos dentro de un contenedor. Es la herramienta de los componentes: barras de navegación, toolbars, tarjetas en fila y centrados. Pregunta: ¿cuántas veces habéis peleado con un centrado vertical esta semana?

---

## Objetivos de aprendizaje

<span class="fragment">1. Activar flexbox y separar <mark>eje principal</mark> y <mark>eje transversal</mark></span>

<span class="fragment">2. Usar <mark>justify-content</mark>, <mark>align-items</mark> y <mark>gap</mark></span>

<span class="fragment">3. Repartir espacio con los factores de flex y su atajo <mark>flex</mark></span>

<span class="fragment">4. Resolver el bloqueo clásico del <mark>min-width: auto</mark> de los items</span>

<span class="fragment">5. Maquetar navbar, centrado total y rejilla de tarjetas que envuelven</span>

Note:
Los cinco objetivos son aplicables desde la primera clase. El cuarto es el que más quebraderos de cabeza causa: «mi flex no se encoge». El quinto os da los tres layouts canónicos que aparecen en cualquier examen o proyecto real. El primero se entiende en diez minutos si dibujáis los ejes.

---

## Motivación · ¿Cuándo se usa flex?

<div style="font-size: 1em; text-align: left;">

Componentes, no páginas: ahí es donde flexbox gana
</div>

<span class="fragment" style="font-size: 1em;">Navbars, toolbars, grupos de botones y tarjetas en fila</span>

<span class="fragment" style="font-size: 1em;">Distribuir o «empujar» espacio sobrante con <mark>margin auto</mark></span>

<span class="fragment" style="font-size: 1em;">Flex es <mark>contenido primero</mark>: el texto define cuánto espacio ocupa</span>

<span class="fragment" style="font-size: 1em;">Para filas y columnas a la vez usamos <mark>grid</mark>, la unidad siguiente</span>

<span class="fragment" style="font-size: 1em;">Flex resuelve el <mark>espacio sobrante</mark> sin absolutos ni elementos flotantes</span>

Note:
La pregunta de diseño es sencilla: ¿distribuyo contenido en una línea que puede envolver? Entonces flex; si necesito filas y columnas a la vez, grid. Los dos modelos conviven en el mismo proyecto, grid fuera y flex dentro. Elegir el modelo equivocado cuesta horas de CSS que se arreglan con una declaración.

---

## Activación: display flex

```css
.contenedor { display: flex; }   /* o inline-flex */
```

<span class="fragment">Los <mark>hijos directos</mark> pasan a ser flex items, aunque sean inline</span>

<span class="fragment">Dejan de funcionar como antes los floats y los márgenes verticales</span>

<span class="fragment">Aparecen dos ejes: <mark>principal</mark> (reparte) y <mark>transversal</mark> (alinea)</span>

<span class="fragment">`inline-flex` crea un contenedor que fluye como un inline dentro del texto</span>

Note:
Con una sola declaración el navegador cambia por completo el algoritmo de layout de los hijos. Recordad que flex solo actúa sobre hijos directos: los nietos no reciben ninguna propiedad del contenedor. `inline-flex` es útil para widgets embebidos en el texto. Además anula el `float` de sus hijos.

---

## Propiedades del contenedor

<span class="fragment"><mark>flex-direction</mark>: `row`, `row-reverse`, `column`, `column-reverse`</span>

<span class="fragment"><mark>flex-wrap</mark>: `nowrap` por defecto, `wrap`, `wrap-reverse`; atajo `flex-flow`</span>

<span class="fragment"><mark>justify-content</mark>: distribuye sobre el eje principal</span>

<span class="fragment"><mark>align-items</mark>: alinea los items sobre el eje transversal</span>

<span class="fragment"><mark>align-content</mark>: alinea las líneas entre sí, solo con `wrap`</span>

<span class="fragment"><mark>gap</mark>: espacio entre items y entre líneas, sin márgenes improvisados</span>

Note:
`flex-direction` es la propiedad que decide cuál es el eje principal, y a partir de ahí todo lo demás depende de ella. `flex-flow` es solo el atajo de `flex-direction` con `flex-wrap`. `gap` ha sustituido a los márgenes entre items porque no genera dobles espacios en los bordes. `align-content` solo actúa cuando hay varias líneas.

---

## Dos reglas fijas sobre los ejes

<span class="fragment"><mark>justify-content</mark> → siempre eje principal</span>

<span class="fragment"><mark>align-items</mark> → siempre eje transversal</span>

<span class="fragment">Valores: `flex-start`, `flex-end`, `center`, `baseline`, `stretch`</span>

<span class="fragment">Reparto: `space-between`, `space-around`, `space-evenly`</span>

<span class="fragment">Con `flex-direction: column` los ejes <mark>se intercambian</mark> de sitio</span>

<span class="fragment">`start` y `end` son las variantes <mark>lógicas</mark> y respetan `direction`</span>

Note:
Si recordáis solo dos frases de la unidad, que sean estas: justify va con el principal y align con el transversal. El error clásico es seguir pensando en horizontal cuando flex-direction es column. Con `space-evenly` el espacio es idéntico en todas partes, bordes incluidos. En textos en árabe o hebreo, preferid `start` y `end`.

---

## Alineación segura con overflow

```css
.contenedor {
  justify-content: safe center;  /* si no cabe, alinea al inicio */
  align-items: safe center;
}
```

<span class="fragment">Con `center` puro y desbordado, el inicio del contenido <mark>queda cortado</mark></span>

<span class="fragment">Ese inicio es inaccesible con teclado o lector de pantalla</span>

<span class="fragment">La variante <mark>safe</mark> cae al inicio solo cuando el contenido no cabe</span>

<span class="fragment">`safe` es una keyword: si no cabe, se alinea al `start` en vez de cortar</span>

Note:
Este es un problema histórico de accesibilidad en flexbox: centrar un contenedor con desbordamiento hace invisible el principio. Las keywords `safe` y `unsafe` resuelven el dilema. Úsalas siempre que centréis algo de altura desconocida. Funciona igual en grid con `place-content: safe center`.

---

## Propiedades de los items

<span class="fragment"><mark>flex-grow</mark>: peso para repartir el espacio sobrante; por defecto `0`</span>

<span class="fragment"><mark>flex-shrink</mark>: peso para quitar espacio si falta; por defecto `1`</span>

<span class="fragment"><mark>flex-basis</mark>: tamaño inicial antes de repartir; por defecto `auto`</span>

<span class="fragment"><mark>order</mark>: cambia el orden visual sin tocar el DOM ni el foco</span>

<span class="fragment"><mark>align-self</mark>: anula `align-items` para un item concreto</span>

Note:
Los tres factores de flex describen el algoritmo de reparto: base, crecimiento y compresión. `order` es potente pero peligroso, porque el lector de pantalla y el Tab siguen el orden del DOM. Usadlo solo para ajustes visuales menores. Su valor por defecto es `0` y admite negativos.

---

## El atajo flex a fondo

| Declaración | Equivale a | Uso típico |
|---|---|---|
| `flex: 1` | `1 1 0%` | Reparto por igual, ignora el contenido |
| `flex: auto` | `1 1 auto` | Crece desde su tamaño natural |
| `flex: none` | `0 0 auto` | Rígido: ni crece ni encoge |

<span class="fragment"><mark>`flex: 1`</mark> no es <mark>`flex: auto`</mark>: la base es cero o contenido</span>

<span class="fragment">`flex: 2 1 0%` dobla el peso de crecimiento de `flex: 1`</span>

Note:
La regla de oro: con `flex: 1` en varios items, los anchos son proporcionales a sus factores. La base `0%` hace que el contenido no tenga voz ni voto. Con `flex: auto` el tamaño natural sí influye y por eso los resultados cambian. Por eso dos paneles con `flex: 1` quedan idénticos aunque su texto no.

---

## Cómo se calcula el tamaño final

<span class="fragment">1. Se fija el <mark>flex-basis</mark>, o `width`/contenido si es `auto`</span>

<span class="fragment">2. Si sobra espacio, se reparte con <mark>flex-grow</mark> proporcionalmente</span>

<span class="fragment">3. Si falta, se quita con <mark>flex-shrink por flex-basis</mark>, no solo con shrink</span>

<span class="fragment">4. Los límites <mark>min-width y max-width</mark> se aplican al final del proceso</span>

<span class="fragment">Por eso un item de base grande <mark>se encoge más</mark> con el mismo factor</span>

Note:
Este algoritmo en cuatro pasos explica casi todos los comportamientos extraños de flex. Fijaos en el paso tres: un item de base grande se encoge más, aunque su factor sea igual. Y el paso cuatro es donde aparece la trampa de la siguiente diapositiva. Los límites mandan al final del algoritmo.

---

## La trampa: min-width auto

```css
.item { flex: 1; min-width: 0; }   /* permite encoger de verdad */
/* alternativa: .item { flex: 1; overflow: hidden; } */
```

<span class="fragment">Los items nacen con <mark>`min-width: auto`</mark>, es decir, su min-content</span>

<span class="fragment">Un texto largo sin espacios <mark>no se encoge</mark> y rompe toda la fila</span>

<span class="fragment">Aparece con URLs, tablas dentro de flex o inputs muy largos</span>

<span class="fragment">La solución explícita es <mark>`min-width: 0`</mark> en ese item</span>

Note:
Este es el problema número uno de flexbox en examen y en producción. El navegador protege el contenido mínimo, y esa protección impide la compresión. Con `min-width: 0` devolvéis el control al flex-shrink y la fila vuelve a caber. En grid el equivalente es `minmax(0, 1fr)`.

---

## Ejemplo · Navbar clásica

```css
.nav {
  display: flex;
  align-items: center;      /* centrado en el eje transversal */
  gap: 1rem;
}
.nav .logo { margin-inline-end: auto; }  /* empuja el resto */
.nav ul { display: flex; gap: .5rem; list-style: none; }
```

<span class="fragment" style="font-size: 1em;">Un <mark>`margin-inline-end: auto`</mark> absorbe todo el espacio libre: efecto push</span>

Note:
Esta navbar cubre logo a la izquierda y menú a la derecha sin una sola coordenada. El truco del margen automático es la forma idiomática de separar grupos en flex. Ojo: sigue respetando la dirección del texto gracias a la propiedad lógica. El mismo truco separa grupos dentro de una toolbar.

---

## Ejemplo · Centrado total y tarjetas

```css
.centro {
  display: flex;
  justify-content: center;   /* eje principal */
  align-items: center;       /* eje transversal */
  min-height: 100dvh;        /* sin esto no hay dónde centrar */
}
.cards { display: flex; flex-wrap: wrap; gap: 1rem; }
.card  { flex: 1 1 250px; }  /* base 250px y crecen por igual */
```

<span class="fragment" style="font-size: 1em;">Con `flex: 1 1 250px` las tarjetas <mark>envuelven</mark> y se reparten el sobrante</span>

Note:
El centrado total solo funciona si el contenedor tiene altura: de ahí el `min-height`. La fila de tarjetas muestra el patrón contenido primero, con una base desde la que empiezan a crecer. Juntos resuelven la mayoría de layouts de componente. Con grid se resuelve en una línea con `place-items: center`.

---

## Mapa · Qué propiedad toca cada eje

```text
flex-direction: row          flex-direction: column
  -> eje principal             | eje principal
  | eje transversal            -> eje transversal

justify-content  reparte en el eje principal
align-items      alinea en el eje transversal
align-content    alinea lineas, solo con wrap
```

Note:
Dibujad este esquema antes de tocar cualquier propiedad: primero identificad el eje principal con flex-direction y ya sabréis si toca justify o align. Con column la flecha vertical es la principal, y por eso centrar en horizontal deja de ser justify-content. Este mapa evita la mitad de los errores de la unidad. Dibujadlo antes de abrir el inspector.

---

## Flex o grid: decisión rápida

<span class="fragment">Flex: <mark>1D</mark>, contenido primero, reparte espacio en una línea</span>

<span class="fragment">Grid: <mark>2D</mark>, layout primero, defines pistas y colocas después</span>

<span class="fragment">Navbar, toolbar, formulario de una fila → <mark>flex</mark></span>

<span class="fragment">Página completa, dashboard, galería con filas → <mark>grid</mark></span>

<span class="fragment">No son rivales: lo habitual es <mark>grid fuera y flex dentro</mark></span>

<span class="fragment">Galerías: grid con `auto-fit` controla las filas; flex con `wrap` también envuelve</span>

Note:
Ambos modelos se complementan, no compiten. Flex brilla cuando el contenido decide el tamaño; grid brilla cuando decides la estructura antes. En un proyecto real usareis grid para el esqueleto de la página y flex dentro de cada componente. Ambos comparten `gap` y las mismas keywords de alineación.

---

## Error común: confundir los ejes

<span class="fragment">⚠ Con `row`, centrar en vertical es <mark>`align-items`</mark>, no `justify-content`</span>

<span class="fragment">⚠ Con `column` los ejes se cambian y el centrado <mark>invierte</mark> de propiedad</span>

<span class="fragment">⚠ Flex solo actúa sobre <mark>hijos directos</mark>, los nietos no se enteran</span>

<span class="fragment">⚠ Item que no encoge y desborda: falta <mark>`min-width: 0`</mark></span>

<span class="fragment">⚠ Sin <mark>`flex-wrap: wrap`</mark> los items se comprimen hasta desaparecer</span>

<span class="fragment">⚠ `order` cambia lo visual, pero DOM, foco y lectores <mark>siguen el original</mark></span>

Note:
Los tres primeros son errores de conceptualización de los ejes, y el cuarto es la trampa del min-width. El último es de accesibilidad: si el orden visual no coincide con el DOM, hay que corregir el HTML, no maquillarlo con CSS. Repasad esta lista antes del examen. La mitad de los fallos son de ejes olvidados.

---

## Autoevaluación

<span class="fragment">1. ¿Qué hace `flex: 1 1 0%` y por qué es distinto de `flex: auto`?</span>

<span class="fragment">2. Diferencia visual entre `space-between`, `space-around` y `space-evenly`</span>

<span class="fragment">3. Una URL larguísima rompe la fila: diagnóstico y arreglo en una línea</span>

<span class="fragment">4. ¿Cómo haces que el tercer item ocupe el doble de espacio que el resto?</span>

<span class="fragment">5. ¿Por qué `order` es peligroso para la accesibilidad?</span>

Note:
Respuestas: uno, reparte con base cero e ignora el contenido; auto parte del tamaño natural. Dos, between pega los extremos, around deja bordes a media distancia y evenly reparte todo por igual. Tres, `min-width: 0` en ese item. Cuatro, `flex-grow: 2` frente a `1`; y `order` es delicado porque DOM, foco y lector no cambian.

---

## Claves para el examen

<span class="fragment">Flexbox es <mark>1D</mark>: un eje reparte, el otro alinea; para filas y columnas → grid</span>

<span class="fragment"><mark>justify-content</mark> va al principal, <mark>align-items</mark> al transversal</span>

<span class="fragment"><mark>`flex: 1`</mark> es `1 1 0%` e ignora el contenido; `flex: auto` es `1 1 auto`</span>

<span class="fragment">Si el item no se encoge, la solución es <mark>`min-width: 0`</mark></span>

<span class="fragment"><mark>`gap`</mark> separa items, `margin: auto` empuja y `order` cambia lo visual</span>

Note:
Cinco ideas clave para el repaso final. Si entendéis los ejes, entendéis toda la unidad; lo demás es memoria de propiedades. Practicad la navbar y el centrado hasta hacerlos de cabeza. Repasad también `flex: 1` frente a `flex: auto`.
