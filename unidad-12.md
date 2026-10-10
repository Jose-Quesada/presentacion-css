# CSS

## Unidad 12 · CSS moderno y arquitectura

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad que da estructura al CSS. Por un lado las características modernas del lenguaje: variables, capas, anidación y alcance; por otro, los patrones de organización que el mercado espera: nomenclatura, tokens y guía de estilo. Aquí se conecta directamente con el RA 1 y el RA 2 del módulo 0615. Pregunta de arranque: ¿cuántos archivos de CSS tiene vuestro proyecto y quién decide el orden?

---

## Objetivos de aprendizaje

<span class="fragment">1. Declarar y consumir <mark>custom properties</mark> con valor de reserva</span>

<span class="fragment">2. Registrar variables animables con <mark>@property</mark></span>

<span class="fragment">3. Ordenar la cascada con <mark>@layer</mark> y detectar soporte con <mark>@supports</mark></span>

<span class="fragment">4. Escribir anidación nativa y <mark>propiedades lógicas</mark> para i18n</span>

<span class="fragment">5. Organizar un proyecto con BEM, tokens y <mark>guía de estilo</mark></span>

Note:
Cinco objetivos, tres de lenguaje y dos de arquitectura. El tercero es el que resuelve las guerras de especificidad de las que todos hemos sufrido. El quinto es el que se evalúa en el trabajo práctico: nomenclatura, tokens y documentación viva.

---

## Motivación · El CSS que nadie se atreve a tocar

<span class="fragment" style="font-size: 1em;">Con varios autores, el CSS plano se vuelve impredecible</span>

<span class="fragment" style="font-size: 1em;">Especificidades que crecen solas y ganan sin motivo</span>

<span class="fragment" style="font-size: 1em;">Código duplicado en cinco hojas y miedo a borrar</span>

<span class="fragment" style="font-size: 1em;">La guía de estilo fija tokens y convenciones</span>

<span class="fragment" style="font-size: 1em;">Interfaces homogéneas: el RA 2 del 0615 lo pide</span>

Note:
La arquitectura no es burocracia: es lo que permite borrar con confianza y encontrar cualquier estilo en segundos. Sin estructura, cada refactor es una lotería. Y además se evalúa, porque la homogeneidad de la interfaz es un resultado de aprendizaje del módulo.

---

## Custom properties · variables nativas

```css
:root {
  --color-marca: oklch(60% 0.2 260);
  --espacio-base: 1rem;
  --radio: .5rem;
}
.boton {
  background: var(--color-marca);
  padding: calc(var(--espacio-base) * .75);
  border-radius: var(--radio);
  color: var(--texto, #111);   /* valor de reserva si no existe */
}
```

- Son la única variable nativa de CSS: se declaran con `--` y se leen con `var()`
- Se heredan como cualquier propiedad y se redefinen por subárbol
- El valor se resuelve donde se usan, no donde se declaran
- JavaScript también las toca con `setProperty` y `getPropertyValue`

Note:
Las custom properties son el cimiento de los design tokens en CSS. Su scope es dinámico, lo que permite theming local sin escribir cascada extra. Fijaos en el valor de reserva: es la red de seguridad frente a cualquier error de tipeo.

---

## Reglas que hay que dominar

- No validan el valor al declararse: el fallo aparece al usarlas
- Un `var()` inválido invalida la declaración completa, en silencio
- `--ColorMarca` y `--colormarca` son dos variables distintas
- No son responsive por sí solas: combínalas con `clamp()` o `@media`
- Tema local: redefinir la variable en el ancestro y el componente no se entera

Note:
El error más común es confiar en que el navegador avisará al definir la variable, y no lo hace. Por eso el valor de reserva es prácticamente obligatorio en equipo. Y ojo con las mayúsculas: son caracteres válidos y crean dos variables diferentes sin ningún aviso.

---

## @property · variables tipadas

```css
@property --angulo {
  syntax: "<angle>";       /* contrato de tipo para el navegador */
  inherits: false;
  initial-value: 0deg;
}
.giro {
  transform: rotate(var(--angulo));
  transition: --angulo 1s;   /* ahora sí interpola */
}
.giro:hover { --angulo: 180deg; }
```

- El tipo registrado es lo que <mark>habilita la animación</mark> de la variable
- Antes, las custom properties solo saltaban de un valor a otro
- Se puede usar donde la propiedad exige un tipo concreto
- Soporte: Chrome 85, Safari 16.4 y Firefox 128

Note:
Antes de @property, cambiar una variable era un corte seco; ahora es una transición normal. El campo syntax es el contrato: angle, color, length, number. El campo inherits decide si el valor baja por el árbol o se queda en el elemento. Este es el detalle que más se pregunta en examen.

---

## Anidación nativa

```css
.tarjeta {
  padding: 1rem;
  &:hover { box-shadow: 0 8px 24px rgb(0 0 0 / .15); }
  &__titulo { font-size: 1.25rem; margin-block-end: .5rem; }
  & > ul { list-style: none; }
  @media (min-width: 40em) {
    display: grid;
    grid-template-columns: auto 1fr;
  }
}
```

- `&` se sustituye por el selector padre en la posición que ocupa
- Solo se anidan reglas y at-rules condicionales dentro de reglas
- El combinador por defecto es el descendiente
- Sin mixins, bucles ni operaciones: eso sigue siendo de Sass

Note:
La anidación nativa trae a CSS la lectura compacta de los preprocesadores, pero con límites deliberados. `&__titulo` genera la clase compuesta al estilo BEM sin escribirla. Y las media queries anidadas son el gran ahorro: la consulta vive junto a la regla que la necesita.

---

## @layer · ordenar la cascada

```css
@layer reset, tokens, base, components, utilities;

@layer reset {
  *, *::before, *::after { box-sizing: border-box; margin: 0; }
}
@layer base { body { line-height: 1.6; } }
@layer components { .boton { padding: .5rem 1rem; } }
@layer utilities { .mt-4 { margin-top: 1rem; } }   /* gana: va la última */
```

- La declaración inicial fija el <mark>orden de prioridad</mark> de todas las capas
- Gana la última declarada: utilities pisa a components
- El CSS sin capa siempre gana a cualquier capa declarada
- Capa añadida sin declarar se coloca al final, con máxima prioridad

Note:
Con @layer el orden lo decidís vosotros y no el orden físico de los archivos. La declaración única al principio del proyecto es la que evita sorpresas. Recordad el matiz contrario: cualquier CSS que no esté capado pisa a todas las capas, por eso se capa todo desde el primer día.

---

## Qué gana qué en la cascada

```text
Menor prioridad                                           Mayor prioridad
      |                                                        |
      v                                                        v
estilos del agente -> capas declaradas -> CSS sin capa -> !important

capas:      reset < tokens < base < components < utilities
```

- Declarad las capas una sola vez y al principio del proyecto
- `revert-layer` permite que una utilidad ceda a la capa anterior
- `@import "base.css" layer(base)` combina importación y capa
- El `!important` sigue invirtiendo el orden: la primera declaración gana

Note:
Este esquema se dibuja en la pizarra cada curso porque resume media unidad. La flecha de las capas es la que se pregunta en examen: de menor a mayor, la última declarada manda. Y recordad que revert-layer es la salida elegante para deshacer una utilidad sin !important.

---

## @supports · mejora progresiva

```css
@supports (display: grid) { .layout { display: grid; } }
@supports (gap: 1rem) { .flex { gap: 1rem; } }
@supports not (backdrop-filter: blur(4px)) {
  .vidrio { background: #fff; }      /* alternativa sólida */
}
@supports (selector(:has(*))) {
  .lista:has(.activo) { border-color: var(--marca); }
}
```

- El fallback se escribe primero y la mejora va dentro del bloque
- Comprueba propiedad, valor, función y negación con `not` y `or`
- `selector()` evita que un navegador antiguo descarte toda la regla
- Es la puerta de entrada de las scroll-driven animations

Note:
@supports es el mecanismo estándar para no romper a nadie mientras se estrena una API. La estructura mental es «antes el básico, después el mejor». Ojo con el `not`: invierte la condición completa, no la propiedad. Practicad con la detección de :has, que es la más útil hoy.

---

## @scope · alcance limitado

```css
@scope (.widget) {
  :scope h3 { margin: 0; }   /* la raíz declarada del alcance */
  a { color: inherit; }      /* no escapa del componente */
  .titulo { font-weight: 600; }
}
/* sin alcance habría que prefijar todo esto */
.widget a { color: inherit; }
```

- Acota las reglas a un árbol sin prefijos ni clases extra
- Evita la especificidad inflada de los selectores larguísimos
- Complementa a la anidación y a los Web Components
- Soporte reciente: conviene verificarlo antes de estrenarlo

Note:
@scope resuelve el viejo problema de que un estilo de componente se cuela en el resto de la página. Con él, las reglas cortas vuelven a ser seguras. El selector :scope señala la raíz declarada, y lo que quede fuera simplemente no se aplica. Es la herramienta más nueva de la unidad.

---

## Propiedades lógicas

| Física | Lógica |
|---|---|
| `margin-left` | `margin-inline-start` |
| `margin-top` | `margin-block-start` |
| `width` / `height` | `inline-size` / `block-size` |

- Siguen el flujo de escritura (i18n), no la pantalla
- Un solo código vale para LTR, RTL y vertical
- `text-align: left` se convierte en `text-align: start`
- Traducir `margin: 1rem 2rem` es pregunta de examen

Note:
Las físicas asumen que el texto empieza a la izquierda, y eso rompe en árabe o hebreo. Las lógicas describen el flujo, así que cambian de lado solas. El ahorro real es doble: menos reglas y cero hojas duplicadas por idioma. Practicad la traducción hasta que salga sin pensar.

---

## Nomenclatura · BEM y alternativas

```text
bloque __elemento --modificador

.tarjeta
.tarjeta__titulo
.tarjeta__titulo--destacado
.tarjeta--compacta
```

- BEM es predecible: se busca y se borra sin efectos laterales
- El elemento no anida más de un nivel semántico
- ITCSS ordena de menor a mayor especificidad: hoy es `@layer`
- Producto: BEM + capas + tokens; prototipo: utility-first

Note:
BEM es el patrón que más se enseña y el que más aparece en ofertas. Su coste son los nombres largos, y con anidación nativa y @scope muchos equipos lo relajan. El criterio de elección es sencillo: si el proyecto va a vivir años, estructura formal; si es un prototipo, velocidad.

---

## Design tokens

```css
/* Origen: tokens.json, generado con Style Dictionary */
:root {
  --color-brand-500: #2563eb;
  --color-danger-500: #dc2626;
  --space-3: 1rem;
  --radius-sm: .375rem;
}
/* Consumo en componentes, sin valores mágicos */
.boton-peligro { background: var(--color-danger-500); }
```

- Decisiones de diseño nombradas como datos: color, espacio, radio, sombra
- Fuente única de verdad compartida con diseño y con apps móviles
- Valor repetido tres veces o más → token con nombre semántico
- Herramientas: Style Dictionary, Tokens Studio o Theme UI

Note:
Un token no es una variable cualquiera: tiene nombre de negocio y está pensado para ser consumido por varias plataformas. El salto de JSON a CSS lo hace el build, y el componente solo ve var(). Si un valor aparece tres veces, ya es token; esa regla corta os evita los cuarenta azules distintos.

---

## Estructura de archivos recomendada

```text
/styles
  ├─ tokens.css       custom properties: color, espacio, radio
  ├─ reset.css        normalización mínima
  ├─ base.css         elementos globales, tipografía, foco
  ├─ layout.css       contenedores y grid de página
  ├─ components/      un archivo por componente
  └─ utilities.css    clases de escape, las mínimas
```

- Cada archivo se importa con su capa en el índice principal
- La guía de estilo documenta tokens, componentes, estados y accesibilidad
- Sin `!important` salvo justificación documentada en la guía
- Commits atómicos por componente: revisar CSS es mucho más fácil

Note:
Este árbol es el mínimo profesional para un proyecto pequeño o medio. La regla es que cada archivo tenga una responsabilidad clara y una capa asignada. La guía de estilo es el documento vivo que acompaña a la estructura: sin ella, la organización se degrada en tres sprints.

---

## Preprocesadores y PostCSS

- Sass sigue vivo por mixins, bucles, funciones y herencia de selectores
- Variables, anidación y capas ya son nativas: hay menos motivo para dependencia
- PostCSS aplica `autoprefixer`, `cssnano` y `postcss-preset-env`
- Pipeline típico: fuentes, Vite o esbuild, CSS final minificado
- Lightning CSS transforma y minifica a velocidad de Rust

Note:
La pregunta de si sigue haciendo falta Sass ya no tiene respuesta única: depende del proyecto. Lo que ha cambiado es que mucho de lo que solo hacía Sass ahora lo hace el navegador. El pipeline moderno se configura casi solo con Vite, y eso baja muchísimo la barrera de entrada.

---

## Ejemplo · migrar a capas y tokens

```css
@layer tokens, components;
@layer tokens {
  :root { --color-brand: oklch(55% 0.2 260); --space-4: 1rem; }
}
@layer components {
  .boton { background: var(--color-brand); padding: .5rem var(--space-4); }
  .boton:hover { background: color-mix(in oklab, var(--color-brand), black); }
}
```

- Colores mágicos pasan a tokens y propiedades lógicas
- `btn-red` pasa a `boton--peligro` con BEM
- Las capas declaradas al inicio acaban con la guerra

Note:
Este es exactamente el ejercicio E11 del banco de prácticas, y sale en el examen de forma parecida. Fijaos en que la migración no cambia el resultado visual, solo su organización. Lo que se valora es la trazabilidad: token para cada valor, capa para cada responsabilidad y BEM para cada nombre.

---

## Ejemplo práctico · Arquitectura con @property, nesting nativo y BEM

```css
/* 1. Variable fuertemente tipada: permite interpolación matemática */
@property --angulo-borde {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}

/* 2. Nesting CSS nativo combinado con metodología BEM */
.tarjeta-producto {
  padding: 1.5rem;
  border-radius: 12px;
  background-color: #ffffff;

  &__titulo {
    font-size: 1.25rem;
    color: var(--color-texto);
  }

  &--destacada {
    border: 3px solid transparent;
    background-image: linear-gradient(#fff, #fff),
                      conic-gradient(from var(--angulo-borde), #3b82f6, #ec4899, #3b82f6);
    background-origin: padding-box, border-box;
    background-clip: padding-box, border-box;
    animation: rotar 3s linear infinite;
  }

  &:hover { transform: translateY(-4px); }
}

@keyframes rotar { to { --angulo-borde: 360deg; } }
```

<span class="fragment"><code>@property</code> dota a las variables de tipo sintáctico permitiendo animar degradados</span>

<span class="fragment">Nesting nativo (<code>&amp;</code>) estructura módulos BEM legibles sin compiladores Sass</span>

<span class="fragment">Arquitectura limpia: separación estricta entre bloque, elemento y modificador</span>

Note:
Arquitectura vanguardista sin dependencias externas:
1. @property: el navegador por defecto trata las CSS variables como texto plano, imposibilitando calcular pasos intermedios en una transición. Al declarar syntax: '<angle>', el motor comprende los grados y puede animar degradados cónicos en tiempo real con @keyframes.
2. Nesting nativo: la especificación CSS oficial ya permite anidar selectores directamente (&__titulo, &--destacada, &:hover), eliminando la necesidad obligatoria de Sass o preprocesadores en muchos proyectos.
3. Metodología BEM: garantiza predictibilidad y especificidad baja constante (0-1-0), facilitando el trabajo escalable en equipos de desarrollo.

---

## Error común: variables y capas sin sistema

⚠ `var(color-marca)` sin `--` muere en silencio

⚠ El fallo de `var()` aparece donde se usa, no al declarar

⚠ Capas sin declarar entran al final y pisan sin avisar

⚠ `--azul1` no dice nada; usa nombres semánticos

⚠ Propiedades físicas dejan el RTL a medias

⚠ Copiar CSS de una librería crea dependencias ocultas

Note:
Los dos primeros son errores de sintaxis que nadie ve porque no hay mensajes en rojo. Los tres siguientes son de arquitectura y aparecen con el tiempo, cuando el proyecto crece. El último es el más caro: código heredado que nadie se atreve a tocar. Revisad vuestro proyecto con esta lista.

---

## Autoevaluación

<span class="fragment">1. Declara `@layer a, b;` y escribe los bloques al revés. ¿Qué regla gana?</span>

<span class="fragment">2. Traduce a lógicas: `top: 0; right: 0; width: 10rem; text-align: left`</span>

<span class="fragment">3. ¿Qué problema de theming resuelve `@property` con `syntax: "<color>"`?</span>

--

**1)** Gana `b`: el orden lo fija la declaración inicial, no el orden físico.

**2)** `inset-block-start: 0` · `inset-inline-end: 0` · `inline-size` · `text-align: start`.

**3)** Da tipo y valor inicial, así que el color se puede transicionar y hereda bien.

Note:
La primera pregunta es la trampa clásica de la unidad: parece que manda el orden de escritura y no es así. La segunda conecta con internacionalización y con el modelo de caja. La tercera une variables tipadas con animación, el hilo que va desde @property hasta el tema de la aplicación.

---

## Claves para el examen

- Las custom properties se heredan y se resuelven donde se usan
- `var()` falla al usarse; por eso existe siempre el valor de reserva
- `@property` añade tipo e inicial y por eso las variables se animan
- `@layer`: orden declarado al principio; gana la última y el CSS sin capa
- Anidación nativa = reglas y at-rules; mixins y bucles siguen siendo de Sass
- Propiedades lógicas: un mismo código para LTR, RTL y escritura vertical

Note:
Seis ideas que cubren las dos mitades de la unidad. Las tres primeras son de lenguaje moderno, las tres últimas de organización. Si os falla el orden de las capas, repasad la diapositiva del esquema antes del examen. El resto se afianza haciendo el ejercicio de migración una vez más.
