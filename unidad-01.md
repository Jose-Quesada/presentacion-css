# CSS

## Unidad 1 · Fundamentos de CSS

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de fundamentos. CSS da aspecto al contenido, pero también impone reglas de convivencia: cascada, especificidad y herencia deciden qué declaración acaba viéndose. Dominar esta unidad es lo que separa un CSS mantenible de un archivo lleno de !important. Pregunta inicial: ¿os ha pasado que un estilo «no os hacía caso»?

---

## Objetivos de aprendizaje

<span class="fragment">1. Explicar la <mark>separación de preocupaciones</mark> entre HTML, CSS y JavaScript</span>

<span class="fragment">2. Escribir la <mark>sintaxis</mark> de una regla y las at-rules frecuentes</span>

<span class="fragment">3. Elegir la forma de inclusión: en línea, interna o <mark>hoja externa</mark></span>

<span class="fragment">4. Resolver conflictos con <mark>cascada</mark>, <mark>especificidad</mark> e herencia</span>

<span class="fragment">5. Validar y depurar con <mark>Jigsaw</mark>, stylelint y DevTools</span>

Note:
Cinco objetivos; el cuarto es el que más suspensos causa en el curso. Dominar cascada y especificidad es lo que separa un CSS ordenado de un archivo plagado de !important. El quinto responde literalmente a un criterio de evaluación del módulo 0615, así que no es opcional.

---

## ¿Por qué existe CSS?

<div style="font-size: 1em; text-align: left;">

CSS describe <mark>cómo se presenta</mark> un documento que ya está estructurado en HTML.
</div>

```text
HTML  -> estructura y semantica (QUE hay en la pagina)
CSS   -> presentacion (COMO se ve)
JS    -> comportamiento (COMO actua)
```

<span class="fragment">A esto el currículo lo llama <mark>separación de preocupaciones</mark> (CE 2.g)</span>

<span class="fragment">Cambiar el diseño <mark>no toca el contenido</mark></span>

Note:
CSS nació para liberar a HTML de la maquetación: antes se maquetaba con tablas y etiquetas presentacionales. La separación de preocupaciones es el principio que sostiene todo lo demás: mantenimiento, accesibilidad y rendimiento. Es exactamente el CE 2.g del módulo 0373, y conviene poder citarlo de memoria.

---

## Ventajas de las hojas de estilo

<span class="fragment"><mark>Mantenibilidad</mark>: un cambio de regla afecta a todos los elementos</span>

<span class="fragment"><mark>Consistencia</mark>: una clase .boton se ve igual en toda la aplicación</span>

<span class="fragment">Menor peso: un CSS que se <mark>cachea una vez</mark> y un HTML más ligero</span>

<span class="fragment"><mark>Accesibilidad</mark>: tamaño de texto, contraste y movimiento reducido</span>

<span class="fragment">Impresión y otros medios con <mark>@media</mark></span>

<span class="fragment">Reutilización: la misma hoja sirve a varios documentos</span>

Note:
Estas seis ventajas son la respuesta completa al CE 2.g. La de consistencia es la que pide el RA 2 del 0615 cuando habla de interfaces homogéneas. La de accesibilidad es la que conecta con el RA 5: sin hojas de estilo no hay adaptación posible al usuario.

---

## Sintaxis · Anatomía de una regla

```css
selector {                              /* selector + bloque */
  propiedad: valor;                     /* declaracion terminada en ; */
  otra-propiedad: valor2 !important;    /* altera la cascada */
}
```

<span class="fragment"><mark>Selector</mark>: a qué elementos llega la regla</span>

<span class="fragment"><mark>Bloque</mark>: llaves con declaraciones de la forma propiedad: valor</span>

<span class="fragment">Comentario con <code>/* así */</code>: no existe comentario de línea única</span>

<span class="fragment">Las <mark>at-rules</mark> empiezan por arroba: @media, @import, @layer</span>

Note:
La sintaxis de CSS es la más simple de las tres tecnologías web: selector más bloque y el navegador hace el resto. Un punto y coma olvidado deja de aplicar el resto del bloque, que es el error de sintaxis más común de todos. El navegador además tolera lo que no entiende: lo ignora y sigue.

---

## At-rules: el mapa general

<span class="fragment"><code>@import</code> importa otra hoja y solo puede ir al inicio de la misma</span>

<span class="fragment"><code>@media</code> aplica estilos condicionales: responsivo, unidad 10</span>

<span class="fragment"><code>@font-face</code> define fuentes personalizadas, unidad 08</span>

<span class="fragment"><code>@keyframes</code> define animaciones y <code>@layer</code> ordena la cascada</span>

<span class="fragment"><code>@supports</code> y <code>@container</code> condicionan por soporte y por contenedor</span>

<span class="fragment"><code>@charset</code> fija la codificación de caracteres en la primera línea</span>

Note:
Todas empiezan por arroba y se reparten por todo el curso: conviene reconocerlas desde ya. Hay dos formas: con bloque, como @media, y con sentencia, como @charset. Si os preguntan qué va al principio de la hoja, la respuesta es @charset y después el @import.

---

## Formas de incluir CSS

<span class="fragment"><mark>En línea</mark>: atributo style, máxima especificidad, no se cachea</span>

<span class="fragment"><mark>Hoja interna</mark>: elemento style en el head o en una sección</span>

<span class="fragment"><mark>Hoja externa</mark>: link con rel stylesheet, la norma del proyecto real</span>

<span class="fragment"><code>@import</code> encadena hojas pero añade peticiones: evítalo</span>

<span class="fragment">El RA 2 distingue <mark>estilos directos</mark> de hojas de estilo externas</span>

Note:
La regla es simple: proyecto real con hoja externa, componente autocontenido con style y valor único con estilo en línea. El currículo evalúa saber cuándo NO usar estilos directos, no solo cuándo usarlos. El @import queda como alternativa, pero por rendimiento conviene repetir el link.

---

## Ejemplo · Tres formas en un documento

```html
<head>
  <!-- 1 · hoja externa: la norma en proyecto real -->
  <link rel="stylesheet" href="/css/base.css">
  <!-- 2 · hoja interna: componentes autocontenidos -->
  <style>.nota { background: #fff8dc; padding: .5rem; }</style>
</head>
<body>
  <!-- 3 · en linea: solo para valor unico dinamico -->
  <p style="color: crimson;">Personalizado</p>
</body>
```

<span class="fragment">La externa se <mark>cachea</mark> y separa totalmente el contenido</span>

<span class="fragment">El orden de los <code>link</code> importa: a igualdad gana la última</span>

Note:
Este documento muestra las tres formas conviviendo, que es exactamente como se ven en proyectos reales. Fijaos en los comentarios, que ordenan el criterio de uso de cada una. En el examen os pedirán decir cuál elegiríais y por qué: la respuesta siempre pasa por caché y mantenibilidad.

---

## Hojas de estilo alternativas

<span class="fragment">Mecanismo clásico: <mark>rel alternate stylesheet</mark> con el atributo disabled</span>

<span class="fragment">JavaScript activa la hoja cambiando su atributo <code>disabled</code></span>

<span class="fragment">Alternativa moderna: <mark>propiedades personalizadas</mark> y data-theme</span>

<span class="fragment">También vale <code>prefers-color-scheme</code> para tema claro y oscuro</span>

<span class="fragment">Es el <mark>CE d</mark> del RA 2 del módulo 0615</span>

Note:
La hoja alternativa nace desactivada y solo se activa si el usuario la pide. Hoy el mercado laboral espera temas construidos con variables CSS, no dos hojas compitiendo. Por eso enseño el mecanismo clásico para el examen y la convención moderna para el trabajo.

---

## La cascada en tres fases

```text
Cascada (tres fases)
|-- 1 · Origen e importancia
|   |-- user-agent < autor < usuario
|   `-- !important del autor < !important del usuario
|-- 2 · Capas @layer -> gana la ultima declarada
`-- 3 · Especificidad -> si empata, gana el orden en el codigo
```

<span class="fragment">Si varias declaraciones chocan en la misma propiedad, decide la <mark>cascada</mark></span>

<span class="fragment">Fase 1: compara <mark>origen e importancia</mark></span>

<span class="fragment">Fase 3: <mark>especificidad</mark> y después orden de aparición</span>

Note:
Estas tres fases se aplican en orden y resuelven cualquier conflicto imaginable. La regla mnemotécnica más útil: el !important del autor vence al normal del usuario, pero el !important del usuario lo aplasta todo. Si no entendéis la cascada, os parecerá que el navegador se equivoca.

---

## Especificidad · Modelo A, B, C

<span class="fragment"><mark>A</mark>: selectores ID, como #cabecera</span>

<span class="fragment"><mark>B</mark>: clases, pseudo-clases y selectores de atributo</span>

<span class="fragment"><mark>C</mark>: tipos de etiqueta y pseudo-elementos como ::before</span>

<span class="fragment">Se comparan de izquierda a derecha y <mark>sin acumulación</mark></span>

<span class="fragment">El estilo en línea queda <mark>fuera de escala</mark> del modelo</span>

Note:
La especificidad es el desempate de la cascada: a igual origen, gana el selector más concreto. Cien tipos no superan a un solo ID porque la comparación se hace componente a componente. El estilo en línea solo se aplica cuando ya no queda nada más que comparar.

---

## Ejemplos de especificidad

| Selector | A-B-C | Comentario |
|---|---|---|
| `h1` | 0-0-1 | un tipo |
| `.tarjeta .precio` | 0-2-0 | dos clases |
| `#menu .enlace:hover` | 1-1-1 | ID, clase y pseudo-clase |

<span class="fragment"><code>:where()</code> aporta <mark>siempre cero</mark> a la especificidad</span>

<span class="fragment"><code>:is()</code> y <code>:has()</code> toman la de su argumento más pesado</span>

Note:
Este cuadro es el que hay que saber reproducir en el examen sin dudar. Recordad que la fila del ID gana a cualquier combinación posible de clases y tipos. Las pseudo-clases funcionales se comportan de forma distinta cada una, y ahí es donde están las preguntas difíciles.

---

## !important, la salida de emergencia

<span class="fragment"><mark>Invierte la cascada</mark>: sube de prioridad sin pasar por fases</span>

<span class="fragment"><mark>No gana siempre</mark>: pierde ante el !important de la hoja de usuario</span>

<span class="fragment">Usos legítimos: anular librerías ajenas o forzar accesibilidad</span>

<span class="fragment">Mal uso: «para que me haga caso» y aparece la <mark>deuda técnica</mark></span>

Note:
Cada !important que añadéis obliga al siguiente a poner otro, y así indefinidamente hasta que nadie entiende quién manda. Si necesitáis uno, el problema es de arquitectura y no de fuerza. La excepción razonable es cuando no podéis tocar el código de un tercero o cuando hay que garantizar el foco visible.

---

## Herencia y palabras clave

<span class="fragment">Se heredan: <mark>color, font-*, line-height, text-align, cursor</mark></span>

<span class="fragment">No se heredan: margin, padding, border, background, display</span>

<span class="fragment"><code>initial</code> vuelve al valor de la especificación y <mark>rompe la herencia</mark></span>

<span class="fragment"><code>inherit</code> toma el valor del padre aunque la propiedad no sea heredable</span>

<span class="fragment"><code>unset</code> combina ambas y <code>revert</code> descarta tu declaración</span>

Note:
Saber qué propiedades se heredan evita repetir la misma regla en cada bloque del documento. La pregunta clásica de examen es diferenciar initial, inherit, unset y revert, y aparece casi siempre. El reset y la normalize son el aplicado práctico de estas palabras clave al arrancar un proyecto.

---

## Valores, funciones y unidades

<span class="fragment">Longitudes: px, rem, em, porcentaje, vw, vh y ch, en la unidad 08</span>

<span class="fragment">Números puros solo en propiedades adimensionales: opacity, z-index</span>

<span class="fragment"><code>calc()</code> permite aritmética mixta entre unidades distintas</span>

<span class="fragment"><code>var(--mi-var)</code> introduce las <mark>propiedades personalizadas</mark></span>

<span class="fragment">Las palabras clave y funciones <mark>no distinguen mayúsculas</mark></span>

Note:
Todo valor dimensional cae en una de estas cuatro categorías: número, longitud, palabra clave o función. La función calc os va a salvar en cualquier maquetación con anchos porcentuales y márgenes fijos. Las variables CSS son el puente con la unidad 12, donde se construyen los design systems.

---

## Validación y depuración

<span class="fragment"><mark>Validador W3C</mark> Jigsaw: sintaxis y propiedades según la especificación</span>

<span class="fragment"><mark>stylelint</mark>: linter configurable que se integra en la integración continua</span>

<span class="fragment"><mark>DevTools</mark>: estilos aplicados, reglas tachadas y especificidad</span>

<span class="fragment">Panel Computed: el <mark>valor final resuelto</mark> por el navegador</span>

<span class="fragment">Esto es el <mark>CE h</mark> del RA 2: herramientas de validación</span>

Note:
Las tres herramientas cubren niveles distintos: Jigsaw valida, stylelint previene y DevTools explica. En el examen hay que citar las tres y saber qué detecta cada una de ellas. Abre las DevTools antes de preguntar por qué un estilo no se aplica: el noventa por ciento se resuelve ahí.

---

## Error común: creer que !important gana siempre

⚠ Pierde ante un **!important** de la hoja de usuario del visitante.

⚠ Dentro del mismo origen sigue sometido a **especificidad** y a **orden**.

⚠ Cada !important obliga a poner otro después: es **deuda técnica**.

⚠ Olvidar el **punto y coma** deja de aplicar el resto del bloque.

Note:
Los dos primeros son el error conceptual más extendido de toda la unidad. El tercero se ve en proyectos antiguos donde cada corrección lleva otro !important encima. El cuarto es de sintaxis y produce el síntoma de «mi CSS no funciona» más frecuente que existe.

---

## Autoevaluación

¿Gana `#menu .enlace` o `nav ul li .enlace:hover`? ¿Y si el primero se escribe `.menu .enlace`?

--

`#menu .enlace` vale (1,1,0) y vence a (0,2,3): un único **ID** supera cualquier número de clases y tipos. Sin el ID, `.menu .enlace` vale (0,2,0) y entonces gana `nav ul li .enlace:hover` por sus **tres tipos**.

Note:
Esta pregunta resume la unidad entera: cascada, especificidad y orden de aparición. Si la entendéis, ya no volveréis a pelearos con el navegador por un estilo que no se aplica. Contestadla en voz alta y comparad con la respuesta antes de pasar de diapositiva.

---

## Claves para el examen

- **Cascada** por fases: origen e importancia → capas → especificidad → orden.
- **Especificidad A-B-C**: ID, clase o pseudo-clase, tipo; sin acumulación.
- `:where()` aporta **cero**; `:is()` y `:has()` la del argumento más pesado.
- `!important` **no** gana siempre: pierde ante el de la hoja de usuario.
- Herencia: `initial` (spec) frente a `inherit` (padre), `unset` y `revert`.
- Inclusión: **link externo** como norma; validar con Jigsaw, stylelint y DevTools.

Note:
Seis claves que resumen los once bloques de la unidad. Las tres primeras son las más preguntadas y la de herencia aparece casi siempre en forma de pregunta directa. Repasadlas con el cuadro de especificidad delante y con las DevTools abiertas.
