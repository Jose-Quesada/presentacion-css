# CSS

## Unidad 8 · Unidades, tipografía y colores

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de unidades, tipografía y colores. Aquí se concentra el bagaje técnico que exige el RA 1: elegir medidas, tipografías y colores con criterio profesional y no a ojo. Es la unidad más transversal: afecta a accesibilidad, rendimiento y diseño a la vez. Pregunta de arranque: ¿en qué unidad está declarado el tamaño del texto de vuestro proyecto?

---

## Objetivos de aprendizaje

<span class="fragment">1. Clasificar las <mark>unidades CSS</mark> y elegir la correcta en cada propiedad</span>

<span class="fragment">2. Aplicar la regla de oro: <mark>rem</mark> para tipografía y px para detalles finos</span>

<span class="fragment">3. Cargar fuentes con <mark>@font-face</mark>, WOFF2 y font-display: swap</span>

<span class="fragment">4. Construir paletas en <mark>oklch</mark> con derivados y contraste AA verificado</span>

Note:
Cuatro objetivos repartidos en las tres partes de la unidad: unidades, tipografía y color. El segundo es el que se comprueba en cualquier ejercicio práctico. El cuarto es el de accesibilidad: una paleta sin contraste verificado suspende la auditoría. El tercero condiciona el rendimiento de carga de la página.

---

## Motivación · Elegir con criterio, no a ojo

<div style="font-size: 1em; text-align: left;">

El RA 1 exige analizar y seleccionar colores y tipografías adecuados para su visualización en pantalla
</div>

<span class="fragment">La tipografía es el 90 % de la interfaz: escala, interlínea y medida</span>

<span class="fragment">Un texto sin contraste AA 4,5:1 es un fallo de accesibilidad</span>

<span class="fragment">Las medidas mal elegidas rompen el zoom del 200 % (WCAG 1.4.4)</span>

<span class="fragment">Una fuente mal cargada provoca saltos de texto y molestias al leer</span>

Note:
No se trata de decorar, sino de que la interfaz se lea bien en cualquier contexto. Las decisiones de esta unidad son auditables: contraste, escalado y tamaño de texto. Quien las domina evita los errores que más veces aparecen en las revisiones. Además, cada acierto aquí mejora la experiencia de todo el alumnado.

---

## Motivación · Lo que se audita en pantalla

<div style="font-size: 1em; text-align: left;">

Tres comprobaciones que se hacen siempre en una revisión de accesibilidad
</div>

<span class="fragment">Texto ampliado al 200 %: nada se sale ni se solapa</span>

<span class="fragment">Contraste del par texto y fondo medido con herramienta</span>

<span class="fragment">Tamaño mínimo de lectura cómoda, de 16 a 20 px en prosa</span>

<span class="fragment">Ninguna decisión mágica escondida en px fijos sin escalar</span>

Note:
Estas cuatro comprobaciones se pueden hacer en cinco minutos con las devtools. Si el proyecto está escrito en rem y con contraste verificado, las supera sin tocar nada. Si falla alguna, el arreglo casi siempre empieza por las unidades y la tipografía. Guardad la lista para vuestros proyectos.

---

## Unidades · clasificación general

| Familia | Ejemplos | Uso típico |
|---|---|---|
| Absolutas | `px`, `cm`, `pt` | `px` es la referencia fija de pantalla |
| Relativas | `em`, `rem`, `ch` | Tipografía y espaciado rítmico |
| Viewport | `vw`, `vh`, `dvh` | Bloques a pantalla completa |

<span class="fragment">Memoriza el núcleo real: px, rem, em, % y vw/vh</span>

<span class="fragment">`fr` solo existe en grid; `cqw` llega con las container queries</span>

<span class="fragment">`ch` mide el ancho de la cifra cero: ideal para prosa y campos</span>

Note:
Toda medida CSS es una magnitud más una unidad. De la lista enorme, el 95 % del CSS real se resuelve con cinco unidades. Distingue siempre absolutas, que no escalan, de relativas, que dependen de otra medida. También hay unidades de tiempo, ángulo y resolución, menos frecuentes pero en la especificación.

---

## px, rem y em · el debate eterno

- `px`: pixel CSS fijo, no respeta la preferencia del usuario
- `rem`: font-size de `:root`, la referencia global de tipografía
- `em`: font-size del propio elemento, hereda y se multiplica
- Regla de oro: <mark>tipografía y espaciado en rem</mark>, bordes finos en px

Note:
El `rem` es el que respeta el tamaño de texto que configure la persona usuaria. El `em` es útil dentro de un componente, pero anidado se multiplica: 1,2em dentro de 1,2em son 1,44 veces. Con `rem` esa sorpresa no existe y el layout aguanta el zoom. Los bordes finos van en px porque no deben engordar al ampliar.

---

## Viewport · el problema del 100vh

```css
.hero { min-height: 100dvh; }   /* llena la pantalla real en móvil */
/* svh: barra visible = mínimo · lvh: barra oculta = máximo */
/* dvh: dinámica, reacciona a la barra → la recomendada */
```

<span class="fragment">En móvil la barra de direcciones aparece y desaparece: <mark>100vh miente</mark></span>

<span class="fragment">`vmin` y `vmax` toman el menor o el mayor de ancho y alto</span>

<span class="fragment">`vw` es útil en tipografía fluida, dentro de un `clamp()`</span>

Note:
Este es el problema clásico de los héroes a pantalla completa: o sobra espacio o se tapa el final. Las unidades dinámicas `svh`, `dvh` y `lvh` nacieron para eso. Para bloques completos en móvil, la respuesta correcta es `100dvh`. En escritorio la diferencia apenas se nota, pero en móvil se ve enseguida.

---

## Porcentajes · ¿respecto a qué?

<span class="fragment">`width` y `height` se calculan sobre el ancho del contenedor padre</span>

<span class="fragment">`margin` y `padding` en % usan también el ancho, ¡también los verticales!</span>

<span class="fragment">`font-size` en % referencia el font-size del padre</span>

<span class="fragment">De ahí el truco antiguo de proporción con padding, hoy `aspect-ratio`</span>

<span class="fragment">`background-position` en % mide la diferencia entre fondo y área</span>

Note:
El porcentaje no es universal: depende de la propiedad de referencia. La trampa más clásica es `padding-bottom: 100%` esperando el alto y conseguir el ancho. Saber eso explica por qué el truco antiguo de la caja cuadrada funciona. Con `aspect-ratio` ya no hace falta ese hack.

---

## Funciones de valor · el diseño fluido

```css
width: min(100% - 2rem, 70rem);        /* fluido con tope */
padding: calc(1rem + 2vw);             /* aritmética mixta */
font-size: clamp(1rem, 2.5vw, 2rem);   /* mín, ideal, máx */
```

- `clamp(min, ideal, max)` crece y decrece <mark>con topes</mark>
- `min()`, `max()` y `calc()` se combinan entre sí sin límite
- `round()` redondea a múltiplos, útil en rejillas de 8 px

Note:
`clamp()` es la herramienta del diseño fluido: un solo valor que se adapta con mínimo y máximo. Con `calc()` mezclas unidades de tipos distintos, como porcentaje y rem. Estas funciones sustituyen a buena parte de las media queries de detalle. Si el valor ideal se sale de los topes, manda el top, así nunca rompe.

---

## Tipografía · familia y métricas

```css
body {
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, Arial, sans-serif;
}
.codigo {
  font-family: ui-monospace, "Cascadia Code", Consolas, monospace;
}
```

- La genérica final es el fallback obligatorio de la lista
- `line-height: 1.5` adimensional escala junto al texto
- `font-variant-numeric: tabular-nums` alinea cifras en tablas
- `text-wrap: balance` reparte los titulares en líneas parejas

Note:
Empieza por `system-ui`: sin descargas y aspecto nativo del sistema. Máximo dos familias por proyecto, display y texto, porque cada variante pesa. De las propiedades olvidadas, `tabular-nums` y `text-wrap: balance` son las dos que más mejora dan por poco esfuerzo. Los nombres con espacios van entre comillas.

---

## @font-face y carga de fuentes

```css
@font-face {
  font-family: "MiFuente";
  src: url("/fonts/mifuente.woff2") format("woff2");
  font-weight: 100 900;          /* rango → fuente variable */
  font-display: swap;            /* fallback visible mientras carga */
  unicode-range: U+0000-00FF;    /* solo subconjunto latino */
}
```

- WOFF2 primero: comprime mejor que WOFF
- `preload` solo de la fuente crítica para evitar saltos (CLS)
- Un archivo variable cubre todos los pesos del rango

Note:
`@font-face` es el contrato entre el CSS y el archivo descargado. Con `swap` el texto nunca desaparece, aunque haya un salto de fuente. Dividir por `unicode-range` evita descargar griego o cirílico si solo usáis latín. El self-hosting da control de privacidad y disponibilidad frente a la CDN.

---

## Escala tipográfica modular

```css
:root {
  --ratio: 1.25;             /* quinta justa */
  --fs-base: 1rem;
  --fs-lg: calc(var(--fs-base) * var(--ratio));
  --fs-xl: calc(var(--fs-lg) * var(--ratio));
}
h1 { font-size: var(--fs-xl); line-height: 1.15; }
p  { font-size: var(--fs-base); line-height: 1.6; max-width: 68ch; }
```

- Ratios de 1,2 a 1,333: más ratio, más jerarquía visual
- Prosa de 16 a 20 px, interlínea 1,5–1,7 y medida de 45 a 75 caracteres

Note:
Una escala modular convierte decisiones sueltas en un sistema coherente. El ratio decide cuánta diferencia hay entre niveles y se guarda como variable. Los títulos llevan interlínea corta y tracking negativo; la prosa, interlínea larga y medida contenida. El ancho de 68ch es la medida óptima de lectura en pantalla.

---

## Colores · formatos modernos

<span class="fragment">Memoriza <mark>oklch()</mark>: luz en %, croma y matiz en grados</span>

<span class="fragment">`hsl()` es intuitivo, pero sus escalas se ensucian al variar la luz</span>

<span class="fragment">`rgb(255 0 0 / .5)` y el `hsl()` moderno admiten alfa con barra</span>

<span class="fragment">`light-dark(#fff, #111)` da los dos temas sin duplicar reglas</span>

<span class="fragment">`transparent` es negro con alfa cero: crea bandas en degradados</span>

Note:
`oklch` es perceptualmente uniforme, así que controlas luz, croma y tono por separado al construir la paleta. El alfa moderno con barra es más legible que la vieja función `rgba`. Conviene memorizar la sintaxis de `oklch`, es la que más se pide. Existen también espacios amplios como P3 para pantallas modernas.

---

## Ejemplo · estados desde un solo token

```css
:root { --marca: oklch(65% 0.2 250); }
.fondo-suave { background: color-mix(in oklab, var(--marca) 10%, white); }
.hover { background: color-mix(in oklab, var(--marca) 85%, black); }
.borde { border: 1px solid color-mix(in srgb, currentColor 30%, transparent); }
```

<span class="fragment">El espacio de mezcla in oklab da transiciones perceptuales suaves</span>

<span class="fragment">Un token y sus derivados: nada de mantener cinco tonos a mano</span>

<span class="fragment">Hover, foco y fondo quedan siempre visualmente coherentes</span>

Note:
`color-mix()` elimina el trabajo manual de derivar hover, activo y foco. El espacio de mezcla importa: `oklab` suaviza y `srgb` es más predecible. Así la paleta queda documentada en tokens y cualquier cambio se propaga sola. Es el mismo principio de las variables CSS aplicado al color.

---

## Ejemplo · tema claro y oscuro sin duplicar

```css
:root {
  --bg: light-dark(#ffffff, #121212);
  --texto: light-dark(#1a1a1a, #eaeaea);
  color-scheme: light dark;   /* controles nativos y barras del tema */
}
```

<span class="fragment">Con `color-scheme` el navegador pinta los formularios del tema</span>

<span class="fragment">Para forzar un tema: selector de atributo con color-scheme dark</span>

<span class="fragment">Combinado con `prefers-color-scheme` cubre cualquier caso de uso</span>

Note:
`light-dark()` devuelve un valor u otro según el tema activo, sin escribir dos bloques. `color-scheme` es lo que hace que la barra de scroll y los campos nativos sigan el tema. Es soportado por los tres motores actuales. Ojo: sin declarar `color-scheme`, algunos navegadores ignoran la preferencia.

---

## Diagrama · Flujo de una paleta accesible

<div style="font-size: 1em; text-align: left;">

`oklch()` define 1 o 2 colores de marca<br>
&nbsp;&nbsp;&nbsp;&nbsp;↓ deriva escalas con `color-mix()`<br>
&nbsp;&nbsp;&nbsp;&nbsp;↓ verifica contraste AA 4,5:1 en cada par<br>
&nbsp;&nbsp;&nbsp;&nbsp;↓ prueba con simulador de daltonismo<br>
&nbsp;&nbsp;&nbsp;&nbsp;↓ documenta como tokens con `light-dark()`

</div>

Note:
Este es el orden de trabajo profesional: primero el token de marca, después las escalas, y la verificación al final. Saltarse el paso de contraste es lo que produce los fallos de auditoría. El último paso, documentar como tokens, conecta con las propiedades lógicas y los temas. Ninguna paleta se da por buena sin la prueba de daltonismo.

---

## Ejemplo práctico · Cotización financiera con tipografía fluida y OKLCH

```css
:root {
  /* Tokens en espacio perceptual OKLCH y color-mix */
  --verde-alza: oklch(60% 0.18 145);
  --fondo-badge: color-mix(in oklab, var(--verde-alza) 15%, transparent);
}

.panel-financiero {
  min-height: 100dvh;  /* Adapta la altura con barras móviles dinámicas */
  padding: clamp(1rem, 2vw + 0.5rem, 3rem);
}

.cifra-valor {
  /* Tipografía fluida sin saltos bruscos de media queries */
  font-size: clamp(1.5rem, 1rem + 2.5vw, 3rem);
  /* Ancho idéntico por dígito: previene temblor de columnas */
  font-variant-numeric: tabular-nums;
  color: var(--verde-alza);
}
```

<span class="fragment"><code>clamp(min, preferido, max)</code> crea escalado tipográfico fluido y continuo</span>

<span class="fragment"><code>tabular-nums</code> alinea decimales en cifras numéricas en tiempo real</span>

<span class="fragment"><code>100dvh</code> garantiza cobertura del viewport sin desfases por barras del navegador</span>

Note:
Diseño visual de precisión con CSS moderno:
1. clamp(1.5rem, 1rem + 2.5vw, 3rem): el titular se expande fluidamente desde pantallas móviles (1.5rem) hasta ultra-panorámicas (3rem) sin un solo breakpoint rígido.
2. font-variant-numeric: tabular-nums: en fuentes proporcionales, el dígito '1' es más estrecho que el '8'. Al actualizar cotizaciones financieras, las columnas tiemblan y bailan. Con tabular-nums todos los dígitos adquieren el mismo ancho monospaciado.
3. OKLCH y color-mix: genera contrastes cromáticos matemáticamente predecibles sin ensuciar la paleta.
4. 100dvh (dynamic viewport height): resuelve el eterno bug de 100vh en móviles cuando la barra de navegación del navegador se oculta al hacer scroll.

---

## Error común: medidas y contraste que no escalan

⚠ `font-size` y `line-height` en px: el texto deja de escalar (WCAG 1.4.4)

⚠ Texto de 16 px con contraste 3:1: incumple AA, el mínimo son 4,5:1

⚠ `padding: 50%` pensando en alto: el % de padding mide el ancho

⚠ Negros y blancos puros `#000` y `#fff`: fatiga visual y halos

⚠ Paleta montada solo en HSL: tonos sucios al subir la luz

Note:
El primero es el error más grave porque rompe el acceso de usuarios con baja visión. El del contraste se detecta con cualquier medidor y se corrige en minutos. Los tres últimos degradan la calidad visual sin que se note de entrada. En el examen, los dos primeros suelen salir como pregunta de corrección de código.

---

## Autoevaluación

<span class="fragment">1. ¿Por qué `rem` es mejor que `em` para el espaciado global?</span>

<span class="fragment">2. ¿Qué altura aplica `padding-top: 50%` en un div de 400 px de ancho?</span>

<span class="fragment">3. Escribe un `@font-face` con fuente variable y `font-display: swap`</span>

--

**1)** `rem` referencia `:root`, así que no se multiplica al anidarse.

**2)** 200 px: los % de padding se calculan sobre el ancho, nunca el alto.

**3)** Con `font-weight: 100 900`, `src` en WOFF2 y `font-display: swap`.

Note:
Las tres preguntas tocan los puntos de la regla de oro, el porcentaje y la carga de fuentes. Si falláis la segunda, revisad la diapositiva de porcentajes: es la trampa clásica del examen. La tercera admite variantes, pero los cuatro elementos son innegociables. Recordad justificar cada decisión, no solo escribir la sintaxis.

---

## Claves para el examen

- Tipografía y espaciado en `rem`; `px` solo para bordes y detalles finos
- En móvil `100vh` miente: la solución correcta es `100dvh`
- Los % de `padding` se calculan sobre el ancho, no sobre el alto
- `clamp(min, ideal, max)` es la tipografía fluida con topes
- `@font-face` con WOFF2, `font-display: swap` y preload de la crítica
- Paleta en `oklch()`, derivados con `color-mix()` y contraste AA 4,5:1

Note:
Estas seis líneas cubren las tres partes de la unidad. Las dos primeras son de unidades, las dos centrales de tipografía y las dos últimas de color. Si las recordáis, el resto son matices de ampliación. Repasad también la tabla de familias de unidades antes del examen.
