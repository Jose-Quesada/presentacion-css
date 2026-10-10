# CSS

## Unidad 3 · Modelo de caja

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad del modelo de caja. Todo lo que el navegador pinta es una caja rectangular y aquí aprendemos a calcular exactamente cuánto mide. De este cálculo dependen el centrado, el espaciado y cualquier layout con flex o grid. Pregunta inicial: ¿alguna vez una caja creció «sola» al añadirle padding?

---

## Objetivos de aprendizaje

<span class="fragment">1. Reconocer las cuatro capas: <mark>contenido, padding, borde y margen</mark></span>

<span class="fragment">2. Calcular el ancho total en <mark>content-box</mark> y en <mark>border-box</mark></span>

<span class="fragment">3. Aplicar <mark>límites</mark>, <mark>aspect-ratio</mark> y elegir <mark>overflow</mark></span>

<span class="fragment">4. Explicar el <mark>colapso de márgenes</mark> y aplicar dos soluciones distintas</span>

<span class="fragment">5. Reproducir los patrones de <mark>centrado</mark>, full-bleed y caja 16:9</span>

Note:
Cinco objetivos, todos prácticos. El segundo y el cuarto son los más examinados: cálculo de ancho y colapso de márgenes. El quinto es el que usaréis cada día en cualquier maquetación real.

---

## Motivación · ¿Por qué medir la caja?

<div style="font-size: 1em; text-align: left;">

Si algo falla en un diseño, casi siempre es un problema de cajas y no de «estética»
</div>

<span class="fragment" style="font-size: 1em;">Un píxel de más de padding <mark>empuja</mark> a todos los vecinos</span>

<span class="fragment" style="font-size: 1em;">Los layouts que «se rompen» suelen ser un <mark>cálculo de ancho</mark> mal hecho</span>

<span class="fragment" style="font-size: 1em;">Es la base para entender <mark>flexbox</mark> y <mark>grid</mark>, que vienen después</span>

<span class="fragment" style="font-size: 1em;">Casi todos los fallos visuales se arreglan <mark>dentro de esta caja</mark></span>

Note:
No es teoría abstracta: el modelo de caja explica la mayoría de desbordes que veis en la web. Además, todo lo que estudiamos aquí se reutiliza en las unidades 05 y 06. Convenceos de que medir bien ahorra horas de maquetación. Un diseñador que domina la caja depura un layout en minutos.

---

## Las cuatro capas de la caja

<span class="fragment"><mark>Contenido</mark>: lo que miden `width` y `height`</span>

<span class="fragment"><mark>Padding</mark>: espacio interior; admite `%` sobre el ancho del padre</span>

<span class="fragment"><mark>Borde</mark>: `border-style` es obligatorio para que se pinte</span>

<span class="fragment"><mark>Margen</mark>: espacio exterior; también admite valores <mark>negativos</mark></span>

<span class="fragment">`outline` no ocupa espacio y `box-shadow` pinta fuera del borde</span>

<span class="fragment">Los atajos `padding-inline` o `margin-block` siguen la <mark>dirección del texto</mark></span>

Note:
Las cuatro capas son concéntricas: de dentro hacia fuera contenido, padding, borde y margen. Recordad que `width` y `height` describen solo el contenido, no la caja completa. Ese detalle es el que provoca casi todos los errores de la unidad. En DevTools veréis las cuatro como anillos de color al pasar el ratón.

---

## box-sizing: content-box vs border-box

- `content-box` (histórico): `width` mide solo el contenido
- `border-box` (estándar): `width` mide la caja entera

| Modo | Ancho real de la caja |
|---|---|
| `content-box` | Contenido + padding + borde (350 px) |
| `border-box` | Solo lo que declara `width` (300 px) |

- El reset `*, *::before, *::after` lo aplica a todo

Note:
`box-sizing` es la propiedad que más cambia el resultado final del CSS. La pregunta de examen clásica es calcular el ancho de una caja en los dos modos: con `width: 300px`, `padding: 20px` y `border: 5px`, una caja mide 350 px en `content-box` y 300 px en `border-box`. Preguntad siempre en qué modo estáis calculando.

--

## Cálculo matemático: content-box vs border-box

```text
Caso: width: 300px; padding: 20px; border: 10px;
```

<span class="fragment"><strong>content-box (suma hacia fuera):</strong><br>
300px + 20px (izq) + 20px (der) + 10px (izq) + 10px (der) = <mark>360px en pantalla</mark></span>

<span class="fragment"><strong>border-box (resta hacia dentro):</strong><br>
La caja mide exactamente <mark>300px en pantalla</mark>.<br>
El contenido interior se encoge a: 300 − 40 − 20 = 240px</span>

<span class="fragment"><strong>¿Por qué rompe layouts content-box?</strong> Dos cajas al 50% con 10px de padding superan el 100% y una cae a la línea siguiente.</span>

Note:
Detalles clave para el aula:
1. Con content-box, añadir padding o borde agranda la caja visualmente.
2. Con border-box, añadir padding o borde encoge el contenido sin alterar el tamaño exterior de la caja.
3. El margen exterior (margin) NUNCA forma parte de width en ninguno de los dos modos.

---

## Ejemplo · Reset universal de caja

```css
/* Reset recomendado en cualquier proyecto */
*, *::before, *::after {
  box-sizing: border-box;  /* width = ancho total de la caja */
}

.tarjeta {
  width: 300px;
  padding: 20px;           /* ya incluida en los 300 px */
  border: 5px solid;       /* ya incluido en los 300 px */
}
```

<span class="fragment" style="font-size: 1em;">Sin el reset, cada componente calcula a su aire y las cuentas dejan de cuadrar</span>

Note:
Estas tres líneas deberían estar en vuestro CSS base desde el primer día. El selector universal cubre también los pseudo-elementos generados. A partir de ahí, `width` significa siempre lo mismo en todo el proyecto. Si usáis una librería de componentes, comprobad que no lo sobrescriba.

---

## Dimensiones: valores de width y height

<span class="fragment"><mark>auto</mark> (por defecto): en horizontal llena el contenedor y en vertical crece</span>

<span class="fragment"><mark>min-content</mark>: el ancho mínimo posible sin romper el contenido</span>

<span class="fragment"><mark>max-content</mark>: el texto entero en una sola línea</span>

<span class="fragment"><mark>fit-content</mark> = min(max(min-content, disponible), max-content)</span>

```css
.etiqueta  { width: fit-content; }  /* se ajusta a su texto */
.columna   { width: min-content; }  /* mínima legible */
```

Note:
`auto` es el valor con el que vivís a diario: los bloques llenan el ancho y crecen en alto. Las palabras clave intrínsecas sirven para ajustar cajas al contenido sin calcular nada. `fit-content` es la fórmula «lo justo» y aparece en muchos ejercicios. `min-content` es el límite que flexbox protege con `min-width: auto`.

---

## Límites: min-max y aspect-ratio

```css
input {
  width: 100%;
  min-width: 12rem;   /* nunca por debajo */
  max-width: 24rem;   /* nunca por encima */
}
```

<span class="fragment"><mark>min-width / min-height</mark> ganan a `width` cuando entran en conflicto</span>

<span class="fragment"><mark>max-width: 100%</mark> en imágenes y media evita desbordes clásicos</span>

<span class="fragment"><mark>aspect-ratio: 16 / 9</mark> con `width: 100%` reserva espacio y evita CLS</span>

Note:
Los límites son la red de seguridad de cualquier caja fluida. Recordad la precedencia: min gana sobre max y ambos ganan sobre el tamaño declarado. Con `aspect-ratio` el navegador reserva la proporción antes de cargar la imagen, evitando saltos de layout.

---

## Márgenes: atajos y propiedades lógicas

```css
.margen {
  margin: 1rem 2rem;             /* vertical horizontal */
  margin: 1rem 2rem 3rem 4rem;   /* arriba derecha abajo izq */
  margin-block: 2rem;            /* arriba + abajo */
  margin-inline-start: 1rem;     /* lado inicial según i18n */
}
```

<span class="fragment">Las propiedades lógicas siguen la <mark>dirección del texto</mark>, no la pantalla</span>

<span class="fragment">Con `dir="rtl"` solo cambian de lado: <mark>cero reglas duplicadas</mark></span>

Note:
El atajo de cuatro valores sigue siendo válido, pero las propiedades lógicas son su versión internacionalizable. En proyectos con varios idiomas, `margin-inline-*` os ahorra hojas de estilo duplicadas. Usad `margin` para lo visual y las lógicas cuando haya dirección de texto. Recordad además que los márgenes horizontales nunca colapsan.

---

## Colapso de márgenes

<div style="font-size: 1em; text-align: left;">

Dos márgenes verticales de bloques que se tocan <mark>no se suman</mark>: colapsan
</div>

<span class="fragment">El valor resultante es el <mark>mayor</mark> de los dos implicados</span>

<span class="fragment">Dos párrafos con `margin: 1rem 0` dejan <mark>1 rem</mark> de hueco, no 2 rem</span>

<span class="fragment">Solo afecta a márgenes <mark>verticales</mark> de bloques en el mismo BFC</span>

<span class="fragment">Los márgenes <mark>horizontales</mark> nunca colapsan</span>

Note:
El colapso es el concepto más examinado del CSS clásico. La regla es corta: verticales, mismo contexto de formato, y nos quedamos con el mayor. Por eso un hueco «mágico» entre párrafos casi nunca es un error de suma, sino un colapso. El síntoma clásico es un espacio que parece más grande de lo que habéis escrito.

--

## ¿Por qué existe el colapso de márgenes?

<span class="fragment"><strong>Origen editorial/tipográfico:</strong> En libros y periódicos, un título con margen inferior de 20px y un párrafo con margen superior de 16px deben separarse 20px, no una zanja de 36px.</span>

<span class="fragment"><strong>Entre hermanos:</strong> Párrafo A (bottom 30px) + Párrafo B (top 20px) $\rightarrow$ hueco real = <mark>Max(30, 20) = 30px</mark></span>

<span class="fragment"><strong>Padre e hijo («el margen que escapa»):</strong> Si el contenedor no tiene borde ni padding superior, el margen del hijo atraviesa al padre y baja todo el contenedor</span>

<span class="fragment"><strong>Regla sagrada de examen:</strong> En horizontal <strong>NUNCA colapsan</strong> (dos botones con 10px de margen siempre dejan 20px).</span>

Note:
Explicación clave para el aula:
El navegador fusiona márgenes verticales que se tocan físicamente.
Para evitar el colapso padre-hijo, la solución moderna recomendada es display: flow-root en el contenedor.

---

## El hijo que empuja al padre

```css
section { }                     /* sin padding-top ni border-top */
section > p:first-child { margin-top: 2rem; }
/* el margen escapa y el section entero baja 2 rem */
```

<span class="fragment">Solución estrella: <mark>display: flow-root</mark>, crea un BFC limpio</span>

<span class="fragment">Alternativas: `overflow: hidden/auto` (pero <mark>recorta</mark>) o `padding-top` mínimo</span>

<span class="fragment">Anulación directa: `section > :first-child { margin-top: 0 }`</span>

Note:
Este es el caso favorito del examen: un hijo con margen que mueve al padre entero. Ocurre porque el margen superior del hijo colapsa con el del padre y se «sale». `display: flow-root` es la solución recomendada porque no tiene efectos secundarios. `overflow: hidden` también crea contexto, pero recorta el contenido.

---

## overflow: pintar, recortar o desplazar

<span class="fragment"><mark>visible</mark> (por defecto): el contenido se desborda pintándose fuera</span>

<span class="fragment"><mark>hidden</mark>: recorta el exceso sin crear barras de scroll</span>

<span class="fragment"><mark>auto</mark>: muestra barras solo cuando hace falta (el más usado)</span>

<span class="fragment">Si un eje no es `visible` y el otro sí, el `visible` pasa a ser <mark>auto</mark></span>

<span class="fragment">Cualquier `overflow` distinto de visible crea un <mark>nuevo BFC</mark> y corta el colapso</span>

Note:
`overflow` decide qué pasa cuando el contenido no cabe en la caja. El detalle contraintuitivo es la conversión de `visible` a `auto` cuando se mezclan ejes. Y recordad: usar overflow para «arreglar» el colapso funciona, pero recorta tooltips y menús desplegables. Para una elipsis en una línea, combinarlo con `text-overflow: ellipsis`.

---

## Ejemplo · Centrado y full-bleed

```css
/* Centrado horizontal de un bloque */
.centrado { margin-inline: auto; max-width: 60rem; }

/* Texto legible: unos 68 caracteres por línea */
.prosa { max-width: 68ch; }

/* Ancho total dentro de un contenedor centrado */
.anchura-total { margin-inline: calc(50% - 50vw); }
```

<span class="fragment" style="font-size: 1em;">Con flex o grid el centrado es aún más corto: lo vemos en las unidades 05 y 06</span>

Note:
Estos tres patrones cubren la mayoría de necesidades de bloque: centrar, medir la línea de texto y romper el contenedor. `margin-inline: auto` sigue siendo la forma clásica de centrar bloques. El truco de `calc(50% - 50vw)` es el famoso full-bleed. Si el padre lleva padding, compensadlo también dentro del `calc`.

---

## Repaso visual · Anatomía de la caja

```text
┌────────────────────────────────┐  <- margen, espacio exterior
│ ┌────────────────────────────┐ │  <- borde, style obligatorio
│ │ ┌────────────────────────┐ │ │  <- padding, espacio interior
│ │ │                        │ │ │
│ │ │      CONTENIDO         │ │ │  <- width / height
│ │ │                        │ │ │
│ │ └────────────────────────┘ │ │
│ └────────────────────────────┘ │
└────────────────────────────────┘
```

Note:
Este esquema es el que dibujaríamos en la pizarra para resolver cualquier ejercicio de la unidad. Situad la propiedad en su capa y el cálculo sale solo. Practicad a reconstruirlo de memoria antes del examen. Después nombrad cada capa sin mirar el esquema.

---

## Contención: contain

```css
.contenedor { contain: content; }  /* layout + paint */
```

<span class="fragment"><mark>`contain: layout`</mark>: lo de dentro no altera el layout de fuera</span>

<span class="fragment"><mark>`contain: paint`</mark>: nada se pinta fuera de la caja, como `overflow: clip`</span>

<span class="fragment"><mark>`contain: strict`</mark> suma layout, paint y size; `content` suma layout y paint</span>

<span class="fragment">⚠ `contain: size` ignora el contenido y exige `contain-intrinsic-size`</span>

Note:
La contención le declara al navegador que ciertas partes del render son independientes y se pueden optimizar. Así las listas largas o los widgets pesados aíslan su impacto sobre el resto de la página. Fijaos en la advertencia de `size`: sin `contain-intrinsic-size` la caja se quedaría vacía. Lo veremos a fondo en la unidad de rendimiento, pero conviene reconocerlo aquí.

---

## Ejemplo práctico · Tarjeta de producto con BFC y aspect-ratio

```css
*, *::before, *::after {
  box-sizing: border-box; /* Padding y border incluidos en el ancho */
}

.tarjeta-producto {
  display: flow-root;    /* Crea BFC: contiene floats y márgenes colapsados */
  max-width: 50ch;       /* Medida óptima de lectura editorial */
  padding: 1.5rem;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  margin-inline: auto;   /* Centrado horizontal lógico */
}

.tarjeta-producto__imagen {
  width: 100%;
  aspect-ratio: 16 / 9;  /* Reserva proporción 16:9 exacta (CLS = 0) */
  object-fit: cover;
  display: block;
}
```

<span class="fragment"><code>box-sizing: border-box</code> hace predecible el dimensionado exacto de las cajas</span>

<span class="fragment"><code>display: flow-root</code> crea un BFC limpio sin recortar tooltips ni sombras</span>

<span class="fragment"><code>aspect-ratio: 16 / 9</code> evita saltos visuales antes de descargar la imagen</span>

Note:
Conceptos clave del modelo de caja aplicados:
1. box-sizing: border-box: evita el cálculo tedioso width + padding + border. El ancho especificado es el tamaño exterior final del elemento.
2. display: flow-root: establece un nuevo contexto de formateo en bloque (BFC). A diferencia de overflow: hidden, no recorta elementos que sobresalgan como dropdowns o sombras exteriores.
3. aspect-ratio: 16 / 9: calcula la altura automáticamente según el ancho disponible, previniendo el desfase acumulado de diseño (Cumulative Layout Shift).
4. max-width: 50ch: limita el ancho a 50 caracteres del glifo '0', respetando las directrices de ergonomía visual y legibilidad.

---

## Error común: sumar márgenes que colapsan

<span class="fragment">⚠ Dos párrafos con `margin: 1rem 0` dejan 1 rem, no 2</span>

<span class="fragment">⚠ El margen del hijo <mark>escapa</mark> sin padding ni borde</span>

<span class="fragment">⚠ `width: 300px` sin <mark>border-box</mark> no son 300 px</span>

<span class="fragment">⚠ `overflow: hidden` <mark>recorta</mark> en vez de arreglar</span>

<span class="fragment">⚠ En flex y grid los márgenes <mark>nunca</mark> colapsan</span>

Note:
Los tres primeros son errores de cálculo y el cuarto es un parche peligroso. El último separa el CSS clásico del moderno: en contenedores flex y grid el colapso no existe. Repasadlo porque mezclar ambos modelos es lo que genera la confusión.

---

## Autoevaluación

<span class="fragment">1. Con `content-box`: `width: 200px`, `padding: 10px`, `border: 2px`. ¿Ancho total?</span>

<span class="fragment">2. Dos párrafos con `margin: 2rem 0` seguidos: ¿qué hueco real hay?</span>

<span class="fragment">3. ¿Qué tres formas conoces de impedir que el margen del hijo escape?</span>

<span class="fragment">4. ¿Qué valor de `overflow` recorta sin crear scroll ni elemento con foco?</span>

<span class="fragment">5. Explica `fit-content` con su fórmula de anidamiento</span>

Note:
Respuestas: uno, 224 px con content-box y 200 px con border-box. Dos, 2 rem porque los márgenes colapsan. Tres, flow-root, overflow distinto de visible o anular el margen del primer hijo. Cuatro, `clip`; y `fit-content` es el mínimo entre el disponible y el máximo.

---

## Claves para el examen

<span class="fragment">Las cuatro capas: <mark>contenido, padding, borde y margen</mark></span>

<span class="fragment">Con <mark>box-sizing: border-box</mark> el `width` declarado es el ancho total</span>

<span class="fragment"><mark>min-/max-</mark> ganan a `width` cuando entran en conflicto</span>

<span class="fragment">Colapso vertical: gana el <mark>mayor</mark>; solución: <mark>display: flow-root</mark></span>

<span class="fragment"><mark>aspect-ratio</mark> con `width: 100%` reserva espacio y evita el CLS</span>

Note:
Cinco ideas para repasar la noche anterior. Si domináis el cálculo de ancho y el colapso de márgenes, tenéis la mitad de la unidad. El resto son patrones que se afianzan practicando en clase. Si falla el colapso, repetid esa diapositiva antes del examen.
