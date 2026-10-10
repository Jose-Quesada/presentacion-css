# CSS

## Unidad 6 · CSS Grid

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Grid es el sistema de maquetación bidimensional: controlas filas y columnas a la vez. Es un modelo layout-first, primero defines la estructura y después colocas el contenido. Es la herramienta por defecto para páginas, secciones, dashboards y galerías. Pregunta: ¿cuántas columnas queréis que tenga vuestro dashboard y quién decide eso?

---

## Objetivos de aprendizaje

<span class="fragment">1. Activar grid y dominar el vocabulario: <mark>pistas, líneas, celdas y áreas</mark></span>

<span class="fragment">2. Definir pistas con <mark>grid-template</mark>, <mark>fr</mark> y <mark>minmax</mark></span>

<span class="fragment">3. Colocar items por líneas o con <mark>grid-template-areas</mark></span>

<span class="fragment">4. Construir galerías responsivas con <mark>auto-fit</mark> y sin media queries</span>

<span class="fragment">5. Alinear con <mark>place-items</mark> y <mark>place-content</mark></span>

Note:
Cinco objetivos que van del vocabulario a los patrones reales. El tercero es el más potente, porque `grid-template-areas` permite rehacer una página entera cambiando solo un mapa de texto. El cuarto es el patrón de galería más usado de la web. El quinto aclara de una vez items frente a content.

---

## Motivación · De una a dos dimensiones

<div style="font-size: 1em; text-align: left;">

Flex reparte en una línea; grid diseña la estructura completa
</div>

<span class="fragment" style="font-size: 1em;">Control de <mark>filas y columnas</mark> a la vez, no una sola dirección</span>

<span class="fragment" style="font-size: 1em;">Ideal para <mark>páginas, dashboards, galerías</mark> y formularios complejos</span>

<span class="fragment" style="font-size: 1em;"><mark>Layout-first</mark>: defines las pistas y luego colocas el contenido</span>

<span class="fragment" style="font-size: 1em;">Se combina con flex: <mark>grid fuera, flex dentro</mark></span>

<span class="fragment" style="font-size: 1em;">En vez de pelear con anchos, se declara la <mark>retícula</mark> una sola vez</span>

Note:
La motivación de grid es que la mayoría de interfaces son cuadriculadas: cabecera, sidebar, contenido, pie. En vez de pelear con anchos y flotar cajas, se declara la retícula una vez. Después, colocar es cuestión de referencias a líneas o a áreas. Y si el cliente cambia el diseño, se edita la plantilla y no las reglas de cada bloque.

---

## Vocabulario: pistas, líneas y áreas

<span class="fragment"><mark>Grid container</mark>: el elemento con `display: grid`</span>

<span class="fragment"><mark>Grid item</mark>: sus hijos directos, y solo ellos</span>

<span class="fragment"><mark>Track o pista</mark>: una fila o una columna</span>

<span class="fragment"><mark>Línea</mark>: el borde de una pista, numerada desde 1</span>

<span class="fragment"><mark>Celda</mark>: intersección de una fila con una columna</span>

<span class="fragment"><mark>Área</mark>: rectángulo formado por cuatro líneas</span>

Note:
Todo el resto de la unidad se expresa con estos seis términos. Las líneas se numeran desde 1 y también admiten negativas, que cuentan desde el final. Si domináis este vocabulario, leer una declaración de grid es trivial. El mapa de líneas de esta unidad los muestra todos a la vez.

---

## Definir la cuadrícula explícita

```css
.g {
  grid-template-columns: 200px 1fr 1fr;  /* 3 columnas */
  grid-template-rows: auto 100px;         /* 2 filas */
}
```

<span class="fragment"><mark>`repeat(3, 1fr)`</mark> equivale a escribir `1fr 1fr 1fr`</span>

<span class="fragment"><mark>`minmax(250px, 1fr)`</mark> fija un mínimo y reparte el resto</span>

<span class="fragment"><mark>`fit-content(200px)`</mark> crece hasta 200 px y ahí se detiene</span>

<span class="fragment">`auto` deja que el <mark>contenido de la celda</mark> decida el tamaño de la pista</span>

<span class="fragment">Lo declarado aquí es la cuadrícula <mark>explícita</mark>; lo demás será implícita</span>

Note:
`grid-template-columns` es la declaración que define la estructura de la sección. Las funciones `repeat`, `minmax` y `fit-content` cubren casi todos los casos reales. Recordad la frontera entre explícita e implícita: solo la explícita la controláis vosotros. Todo lo demás lo genera el navegador.

---

## La unidad fr y los gaps

```css
grid-template-columns: 1fr 2fr;  /* de 900px: 300 y 600 */
gap: 1rem;                       /* ni pista ni línea */
```

<span class="fragment">`fr` reparte <mark>lo que sobra</mark> tras restar pistas fijas y gaps</span>

<span class="fragment">Si no queda nada, un `1fr` se resuelve a <mark>0</mark></span>

<span class="fragment"><mark>`minmax(0, 1fr)`</mark> evita que el contenido mínimo inflé la pista</span>

<span class="fragment">El `gap` se resta una sola vez, sin dobles huecos en los bordes</span>

<span class="fragment">Mezclar fijos y fr da <mark>fijo más reparto</mark>: `250px 1fr 2fr`</span>

Note:
`fr` no es un porcentaje: es la fracción del espacio libre disponible. Las pistas fijas se resuelven primero y después se reparte lo que queda. Por eso `1fr 1fr 1fr` no siempre son exactos tercios cuando hay gaps. La pareja `minmax(0, 1fr)` es el equivalente en grid del `min-width: 0` de flexbox.

---

## Cuadrícula implícita y auto-flow

```css
.g {
  grid-template-columns: repeat(3, 1fr); /* explícita */
  grid-auto-rows: 120px;      /* altura de las filas implícitas */
  grid-auto-flow: row;        /* row | column | dense */
}
```

<span class="fragment">Si hay más items que celdas, el navegador crea <mark>pistas implícitas</mark></span>

<span class="fragment"><mark>`grid-auto-rows`</mark> fija la altura de esas filas de más</span>

<span class="fragment"><mark>`grid-auto-columns`</mark> hace lo propio con las columnas implícitas</span>

<span class="fragment"><mark>`dense`</mark> rellena huecos anteriores pero cambia el orden visual</span>

Note:
La cuadrícula implícita evita que el contenido que sobra se desborde. `dense` es útil para mosaicos, pero introduce una diferencia entre orden visual y orden del DOM. En ese caso conviene documentarlo o evitarlo por accesibilidad. También altera el orden del Tab.

---

## Colocar items por líneas

```css
.item {
  grid-column: 1 / 3;    /* líneas 1 a 3: dos columnas */
  grid-row: 2;           /* una línea: fila 2 */
}
.ancho { grid-column: 1 / -1; }  /* todo el ancho */
```

<span class="fragment"><mark>`span 2`</mark> avanza dos pistas desde la línea actual</span>

<span class="fragment">Las líneas <mark>negativas</mark> cuentan desde el final de la cuadrícula</span>

<span class="fragment">Si omites un lado, el otro se calcula automáticamente con `auto`</span>

Note:
Colocar por líneas es preciso pero hay que contar. El truco `1 / -1` aparece en todos los layouts para ocupar el ancho completo. También se pueden nombrar las líneas, lo que hace el CSS mucho más legible en proyectos grandes. Los nombres admiten repeticiones cuando se usa `repeat`.

---

## Alineación: contenido frente a pistas

<span class="fragment"><mark>`place-items`</mark> → el contenido dentro de su celda</span>

<span class="fragment"><mark>`place-content`</mark> → el conjunto de pistas del contenedor</span>

<span class="fragment"><mark>`place-items: center`</mark> centra en ambos ejes</span>

<span class="fragment">`justify-self`/`align-self` anulan para un item</span>

<span class="fragment">Valores habituales: `start`, `end`, `center` y `stretch`</span>

<span class="fragment">También `space-between` y `space-evenly`</span>

Note:
Esta distinción es la que más confunde: items es contenido en celda y content es el bloque de pistas dentro del contenedor. En la práctica, `place-items: center` es el centrado absoluto más corto de todo CSS. `place-content` solo actúa cuando las pistas no ocupan todo el espacio. `place-self` anula el valor para un solo item.

---

## Ejemplo · Página con grid-template-areas

```css
.pagina {
  display: grid;
  grid-template-areas:
    "cabecera cabecera cabecera"
    "nav      main     aside"
    "pie      pie      pie";
  grid-template-columns: 200px 1fr 250px;
}
.cabecera { grid-area: cabecera; }
```

<span class="fragment" style="font-size: 1em;">El responsive cambia <mark>solo el mapa de texto</mark> dentro de la media query</span>

Note:
Este patrón dibuja la página como si fuera un mapa: cada hueco tiene nombre y cada zona se asigna con `grid-area`. Reordenar la maqueta es editar esas líneas de texto, sin tocar las reglas de los componentes. Es la forma más mantenible de maquetar una página. Las áreas deben formar rectángulos completos.

---

## Galería fluida sin media queries

```css
.galeria {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
  gap: 1rem;
}
```

<span class="fragment">Crea tantas columnas de <mark>mínimo 240 px</mark> como quepan en el contenedor</span>

<span class="fragment">El sobrante lo reparte `1fr`: la columna se adapta <mark>solo</mark></span>

<span class="fragment">Variante segura: `minmax(min(240px, 100%), 1fr)` para paneles muy estrechos</span>

Note:
Esta es la declaración responsiva más famosa de CSS moderno: una sola línea y no hace falta ninguna media query. El `min()` del mínimo protege contra desbordes en contenedores estrechos. Merece la pena memorizarla tal cual. Vale igual para tarjetas, listas o iconos.

---

## auto-fill frente a auto-fit

| Valor | Qué hace con las vacías |
|---|---|
| `auto-fill` | Las reserva: huecos para nuevos items |
| `auto-fit` | Las colapsa: estira las ocupadas |

<span class="fragment">Columnas: <mark>`min·n + gap·(n − 1) ≤ ancho`</mark></span>

<span class="fragment">Sin gaps en la cuenta salen <mark>columnas fantasma</mark></span>

<span class="fragment">La fórmula vale para cualquier `minmax` y `gap`</span>

Note:
Pregunta de examen clásica: cuántas columnas caben con un minmax y un gap dados. `auto-fill` piensa en el futuro y deja huecos; `auto-fit` estira lo que hay. Con el ejemplo de 200 px y 20 px de gap en 1000 px caben exactamente cuatro columnas.

--

## ¿Cuándo usar auto-fit y cuándo auto-fill?

<span class="fragment"><strong>El caso clave:</strong> Solo tienes 2 tarjetas en una pantalla panorámica de 4K donde cabrían 6 columnas de 250px.</span>

<span class="fragment"><strong>auto-fit (colapsa huecos a cero):</strong> Las columnas vacías desaparecen y las 2 tarjetas se estiran ocupando el 50% de la pantalla cada una. <mark>Es lo deseado en el 95% de galerías.</mark></span>

<span class="fragment"><strong>auto-fill (mantiene huecos creados):</strong> Las 2 tarjetas miden sus 250px a la izquierda y la derecha queda vacía esperando nuevos elementos.</span>

<span class="fragment"><strong>Truco anti-desborde móvil:</strong> <code>minmax(min(100%, 250px), 1fr)</code> evita scroll horizontal si la pantalla mide menos de 250px.</span>

Note:
Explicación clave para el aula:
1. auto-fit adapta y ensancha los elementos presentes.
2. auto-fill llena la cuadrícula con celdas invisibles que no se estiran.
3. La fórmula matemática es: min·n + gap·(n − 1) ≤ ancho disponible.

---

## Subgrid

```css
.tarjeta {
  display: grid;
  grid-template-rows: subgrid;  /* hereda las filas del padre */
  grid-row: span 3;
}
```

<span class="fragment">Un item <mark>hereda la definición de pistas</mark> de su contenedor padre</span>

<span class="fragment">Los títulos de todas las tarjetas quedan <mark>alineados</mark> sin alturas fijas</span>

<span class="fragment">Requiere que el item sea también grid container en ese eje</span>

Note:
Subgrid resuelve el problema de alinear el interior de varias tarjetas cuando el texto varía en longitud. Sin él, haría falta fijar alturas a mano con hacks. Es soporte moderno y aparece cada vez más en ejercicios y proyectos. El item hereda las pistas solo en el eje declarado.

---

## Ejemplo · Dashboard de doce columnas

```css
.dash {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-auto-rows: minmax(100px, auto);
  gap: 1rem;
}
.kpi    { grid-column: span 3; }
.graf   { grid-column: span 8; grid-row: span 2; }
.tabla  { grid-column: span 12; }
```

<span class="fragment" style="font-size: 1em;">Las <mark>12 columnas</mark> son la base de casi todos los sistemas de diseño</span>

Note:
Con una retícula de doce columnas, cada bloque ocupa un múltiplo y todo queda alineado por construcción. `grid-auto-rows` con `minmax` da una altura mínima estable sin bloquear el crecimiento. Es el patrón de dashboard que se pide en los exámenes. Con `span` repartís los 12 huecos del sistema.

---

## Mapa · Líneas, pistas y celdas

```text
   linea1      linea2      linea3      linea4
     +----------+----------+----------+
     |  celda   |  celda   |  celda   |  fila 1
     +----------+----------+----------+
     |  celda   |  celda   |  celda   |  fila 2
     +----------+----------+----------+

grid-column: 1 / 3   -> dos primeras columnas
grid-row: 2          -> la segunda fila
```

Note:
Dibujad esta retícula antes de resolver cualquier ejercicio de colocación: numerad las líneas y ya podéis escribir `grid-column` sin dudar. Recordad que las líneas negativas cuentan hacia atrás desde el final. Con el mapa delante, colocar por líneas o por áreas es directo.

---

## Ejemplo práctico · Dashboard responsivo con Grid y auto-fit

```css
.dashboard {
  display: grid;
  /* Rejilla fluida sin media queries y a prueba de móviles estrechos */
  grid-template-columns: repeat(auto-fit, minmax(min(100%, 15rem), 1fr));
  gap: 1.5rem;
}

.kpi-destacado {
  grid-column: span 2;  /* Se expande a dos columnas si caben */
}

.kpi-resumen {
  display: grid;
  place-items: center;  /* Centrado horizontal y vertical en una línea */
  min-height: 120px;
}
```

<span class="fragment"><code>auto-fit</code> calcula y expande dinámicamente las columnas según el espacio</span>

<span class="fragment"><code>min(100%, 15rem)</code> previene el desbordamiento horizontal en pantallas &lt; 240px</span>

<span class="fragment"><code>place-items: center</code> centra contenidos bidimensionalmente de forma concisa</span>

Note:
La fórmula maestra del diseño moderno con CSS Grid:
1. repeat(auto-fit, minmax(min(100%, 15rem), 1fr)): genera tantas columnas de al menos 15rem como quepan en la pantalla y reparte el espacio sobrante por igual (1fr). Si no hay espacio, envuelve a la siguiente fila sin necesidad de @media queries.
2. La función min(100%, 15rem) actúa como salvaguarda: si un móvil tiene una pantalla de 300px (menor que 15rem = 240px), la columna nunca superará el 100% del ancho del viewport, eliminando el scroll horizontal accidental.
3. grid-column: span 2: hace que los widgets prioritarios (KPIs, gráficos) ocupen el doble de ancho siempre que el layout disponga de al menos dos columnas.
4. place-items: center: atajo sintáctico de align-items y justify-items para centrar métricas y cifras.

---

## Error común: fr, auto-fit y nodos sueltos

<span class="fragment">⚠ Esperar que `fr` ignore el contenido: usa <mark>`minmax(0, 1fr)`</mark></span>

<span class="fragment">⚠ `auto-fit` colapsa y `auto-fill` reserva: <mark>no los mezcles</mark></span>

<span class="fragment">⚠ Grid solo ve <mark>hijos directos</mark>: envuelve el resto o usa subgrid</span>

<span class="fragment">⚠ Espaciar con márgenes en vez de <mark>`gap`</mark>: dobles huecos en los bordes</span>

<span class="fragment">⚠ `dense` sin pensarlo: el orden visual deja de coincidir con el DOM</span>

Note:
Los dos primeros son los más frecuentes en examen. El tercero explica por qué «los estilos no le llegan» a un componente anidado. Y el cuarto es la diferencia entre maquetación antigua y moderna: en contenedores grid, el espaciado se declara con gap. El último caso mezcla el orden visual con el del DOM, que es lo que leen los lectores de pantalla.

---

## Autoevaluación

<span class="fragment">1. ¿Cuántas columnas caben con `minmax(200px, 1fr)` y `gap: 20px` en 1000 px?</span>

<span class="fragment">2. Diferencia entre `grid-column: 2 / 4` y `grid-column: 2 / span 2`</span>

<span class="fragment">3. ¿Por qué `minmax(0, 1fr)` arregla el desborde de un contenido largo?</span>

<span class="fragment">4. Explica subgrid con el ejemplo de las tarjetas con títulos alineados</span>

<span class="fragment">5. ¿Cuándo usarías `auto-flow: dense` y qué coste de accesibilidad tiene?</span>

Note:
Respuestas: uno, cuatro columnas porque 200·n + 20·(n − 1) ≤ 1000 da n igual a 4. Dos, nada: ambas ocupan dos pistas desde la línea 2. Tres, porque permite que la pista baje de su mínimo de contenido. Cuatro, el item hereda las filas del padre con `grid-template-rows: subgrid`; y `dense` sirve en mosaicos, pero cambia el orden frente al DOM.

---

## Claves para el examen

<span class="fragment">Grid es <mark>2D y layout-first</mark>: defines las pistas y después colocas el contenido</span>

<span class="fragment"><mark>`fr`</mark> reparte lo que sobra; sin inflarse: <mark>`minmax(0, 1fr)`</mark></span>

<span class="fragment"><mark>`grid-column: 1 / -1`</mark> ocupa todo el ancho y `span n` avanza n pistas</span>

<span class="fragment"><mark>`auto-fit`</mark> colapsa columnas vacías y <mark>`auto-fill`</mark> las reserva</span>

<span class="fragment">Convivencia: <mark>grid fuera y flex dentro</mark>; `place-items: center` centra todo</span>

Note:
Cinco ideas para cerrar la unidad. Las dos más preguntadas son la fórmula de auto-fit con gaps y la diferencia entre fit y fill. Con `grid-template-areas` y la galería fluida tenéis los dos patrones que más se repiten en la práctica. Repasad la fórmula de columnas con gaps.
