# CSS

## Unidad 13 · Accesibilidad y usabilidad

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de accesibilidad y usabilidad. El módulo 0615 le dedica dos resultados de aprendizaje enteros, el RA 5 de interfaces accesibles y el RA 6 de interfaces amigables. Veremos los criterios WCAG que el CSS implementa directamente y las heurísticas de usabilidad aplicadas al estilo. Pregunta de arranque: ¿habéis probado algún día vuestro proyecto solo con el teclado?

---

## Objetivos de aprendizaje

<span class="fragment">1. Situar el trabajo en el marco <mark>WCAG</mark> y su nivel de conformidad AA</span>

<span class="fragment">2. Verificar contraste de texto <mark>4.5:1</mark> y de componentes 3:1</span>

<span class="fragment">3. Mantener <mark>foco visible</mark> sin anular la outline sin más</span>

<span class="fragment">4. Elegir la técnica correcta para <mark>ocultar contenido</mark></span>

<span class="fragment">5. Traducir heurísticas de <mark>Nielsen</mark> en decisiones de CSS</span>

Note:
Cinco objetivos, tres de cumplimiento y dos de criterio de diseño. El cuarto es el que más preguntas genera en examen, porque las cuatro técnicas parecen iguales y no lo son. El quinto conecta la accesibilidad con la calidad percibida del producto.

---

## Motivación · No es un extra

<div style="font-size: 1em; text-align: left;">

Es requisito de titulación, obligación legal en muchos contextos y condición de contrato
</div>

<span class="fragment" style="font-size: 1em;">Directiva europea de accesibilidad y legislación nacional de discapacidad</span>

<span class="fragment" style="font-size: 1em;">Los contratos públicos piden declarar y verificar la conformidad AA</span>

<span class="fragment" style="font-size: 1em;">Un teclado sin foco visible deja la web inutilizable para muchos usuarios</span>

<span class="fragment" style="font-size: 1em;">Mejora para todo el mundo: texto legible, objetivos grandes, estados claros</span>

Note:
La accesibilidad se percibe como trámite y es justo al revés: es lo que separa un prototipo de un producto entregable. Además, gran parte del trabajo es CSS bien escrito, no tecnología especial. Y lo que ayuda a una persona con discapacidad motriz ayuda a quien va con prisa en el móvil.

---

## Motivación · Para quién lo hacemos

<span class="fragment">Personas con baja visión o daltonismo: contraste y no solo color</span>

<span class="fragment">Quien navega con lector de pantalla: árbol de accesibilidad y foco</span>

<span class="fragment">Personas con vértigo o sensibilidad vestibular: movimiento reducido</span>

<span class="fragment">Quien usa el teclado exclusivamente o un teclado de seguimiento</span>

<span class="fragment">Y también quien amplía el texto al 200 % o lee con luz de fondo</span>

Note:
El error es pensar en «el usuario con discapacidad» como un caso aparte. Las preferencias del sistema existen precisamente porque las necesidades cambian según el día y el contexto. Cuando diseñáis para los extremos, el caso medio mejora solo. Ese es el argumento práctico de la universalidad.

---

## Marco · los cuatro principios POUR

<span class="fragment">Perceptible: se ve o se escucha, con contraste y alternativas</span>

<span class="fragment">Operable: funciona con teclado, foco y objetivos alcanzables</span>

<span class="fragment">Comprensible: lenguaje claro, comportamiento predecible</span>

<span class="fragment">Robusto: HTML y CSS válidos, compatible con tecnologías de apoyo</span>

<span class="fragment">Niveles: A mínimo, <mark>AA</mark> el exigido y AAA parcial por apartados</span>

Note:
POUR es el acrónimo que resume la especificación entera y siempre cae en examen. El nivel AA es el que piden los contratos y las prácticas profesionales, porque AAA no es viable en todo un sitio. Cada criterio tiene número propio, y el más preguntado es el 1.4.3 de contraste.

---

## Contraste de texto · criterio 1.4.3

```css
/* Bien: 14.7:1 sobre blanco */
:root { --texto: #1f2937; --fondo: #ffffff; }
/* Mal: gris sobre gris, ~1.9:1, fuera de AA */
.aviso { color: #9ca3af; background: #f3f4f6; }
/* Componentes de UI: mínimo 3:1 (1.4.11) */
input { border: 2px solid #6b7280; }
```

- Texto normal: mínimo <mark>4.5 : 1</mark>; texto grande: 3:1
- Texto grande: 24 px o 18.66 px en negrita
- El ratio se calcula sobre el fondo efectivo, no el teórico
- `opacity` sobre texto baja el contraste real: medidlo, no lo supongáis

Note:
El ratio compara luminancias relativas, y en la práctica se mide con una herramienta. El truco de los grises claros es el fallo más frecuente en proyectos reales: parece elegante y no pasa AA. En modo oscuro la regla es idéntica, no asumáis que oscuro significa más contraste.

---

## El color nunca va solo

```html
<!-- Mal: solo el rojo comunica que hay error -->
<span style="color: red">Error</span>
<!-- Bien: icono + texto, el color solo refuerza -->
<span class="error"><svg aria-hidden="true">✕</svg> Falta el email</span>
```

- Criterio 1.4.1: el color <mark>no puede ser la única</mark> señal de información
- Añade icono, texto, subrayado o patrón al estado de error
- Los enlaces se distinguen también con subrayado, no solo con color
- Pruebad con un simulador de daltonismo: la deuteranopía es la más frecuente

Note:
Este criterio es de nivel A, o sea, el mínimo absoluto. El error clásico es un formulario donde solo el borde rojo indica el campo mal escrito. El mismo razonamiento aplica a los enlaces dentro del texto: quitar el subrayado y dejar solo el color es una decisión arriesgada.

---

## Foco visible · criterios 2.4.7 y 2.4.11

```css
:focus-visible {
  outline: 3px solid var(--color-enfoque);
  outline-offset: 2px;
}
/* El header fijado no debe tapar el campo enfocado */
section[id] { scroll-margin-top: 5rem; }
```

- Nunca `outline: none` sin un sustituto igual o mejor
- El indicador debe verse contra cualquier fondo: el color sale de un token
- El header fijo tapa el foco: ahí entra `scroll-margin-top`
- `opacity: 0` no oculta el foco: el elemento sigue siendo enfocable

Note:
La outline existe para eso, para marcar el foco, y quitarla sin más deja a la navegación por teclado a ciegas. `:focus-visible` permite estilizar solo cuando el foco viene de teclado, sin ensuciar los clics con ratón. El segundo criterio, el del foco tapado, es el que aparece con headers fijos y modales.

---

## Zoom, reflow y espaciado

<span class="fragment">1.4.4: el texto escala al 200 % sin perder contenido ni función</span>

<span class="fragment">Trabajad en `rem` y evitad alturas fijadas con `overflow: hidden`</span>

<span class="fragment">1.4.10: a 320 px no debe haber scroll en dos dimensiones</span>

<span class="fragment">Las tablas anchas admiten scroll interno en su contenedor, no en la página</span>

<span class="fragment">1.4.12: con line-height 1.5 y word-spacing 0.16em no se pierde nada</span>

Note:
Estos tres criterios comparten una causa raíz: las cajas de altura fija sobre texto. Si la medida está en rem y el bloque crece, el resto sale solo. El de espaciado es el menos conocido, y sorprende porque el usuario puede forzar esos valores desde su perfil del sistema.

---

## Hover, foco y movimiento

<span class="fragment">1.4.13: lo que aparece al pasar debe poder cerrarse con Escape</span>

<span class="fragment">Debe ser sostenable con el puntero: no desaparecer al acercarse</span>

<span class="fragment">Y fijable con un clic si la información es crítica</span>

<span class="fragment">2.3.3: las animaciones por interacción se pueden desactivar</span>

<span class="fragment">Nada debe parpadear más de tres veces por segundo (2.3.1)</span>

Note:
Los tooltips y menús desplegables son los que más fallan: desaparecen antes de que el dedo llegue. La solución de diseño es una zona muerta generosa y una salida corta. En cuanto al movimiento, el bloque de prefers-reduced-motion de la unidad 11 es la implementación de este criterio.

---

## Ocultar contenido

| Técnica | Árbol a11y | Caso |
|---|---|---|
| `display: none` | No | Menú cerrado |
| `opacity: 0` | Sí, enfocable | Oculta y ocupa |
| `.visually-hidden` | Sí, legible | Solo lectores |
| `visibility: hidden` | No | Reserva hueco |

- `visibility: hidden` reserva hueco y sale del árbol
- `opacity: 0` aún responde al Tab: activa lo invisible
- Contra eso: `display: none` o el atributo `inert`

Note:
Esta tabla es la pregunta de examen fija de la unidad. La distinción clave es si el elemento sigue en el árbol de accesibilidad. `.visually-hidden` es la única que deja el contenido legible sin mostrarlo en pantalla. Practicad diciendo en voz alta qué caso de uso corresponde a cada una.

---

## Ejemplo · texto solo para lectores

```html
<a class="visually-hidden" href="#contenido">Saltar al contenido</a>
<header>… menú de navegación …</header>
<main id="contenido">…</main>
```

```css
.visually-hidden {
  position: absolute !important;
  width: 1px; height: 1px; margin: -1px;
  padding: 0; overflow: hidden;
  clip-path: inset(50%); white-space: nowrap;
}
```

- El enlace existe para el lector y aparece al recibir el foco
- Sin él, la navegación por teclado se atasca en el menú en cada página
- Sirve también para dar nombre accesible a un icono sin texto visible

Note:
Es el patrón de accesibilidad más reutilizable del CSS moderno. Fijaos en el clip-path y en el ancho de un píxel: el contenido sigue siendo legible para el lector de pantalla pero no ocupa sitio. El `!important` evita que un estilo de componente lo rompa por accidente.

---

## Ajustes del sistema operativo

```css
@media (forced-colors: active) {
  .boton { border: 1px solid ButtonText; }
  .icono { forced-color-adjust: none; }   /* solo si el icono ES el dato */
}
@media (prefers-contrast: more) {
  .campo { border-width: 2px; }
}
```

- Con colores forzados el sistema pinta su paleta: aportad estructura
- Si antes un color marcaba el estado, ahora hace falta un borde
- También se respetan `prefers-reduced-motion` y `prefers-color-scheme`
- Se emulan en DevTools, pero probad el ajuste real del sistema

Note:
El alto contraste de Windows sustituye vuestros colores por una paleta forzada, y los elementos sin borde desaparecen. La regla mental es sencilla: allí donde el color daba información, ahora debe haber estructura. Y recordad que el modo oscuro del usuario manda siempre sobre vuestro CSS.

---

## Usabilidad · las 10 heurísticas de Nielsen

- Visibilidad del estado: cargando, éxito y error se ven sin preguntar
- Lenguaje del usuario: etiquetas claras, sin jerga interna
- Control y libertad: Escape, cancelar y deshacer siempre disponibles
- Consistencia: mismo componente, mismo comportamiento en todo el sitio
- Prevención y recuperación de errores: validación y mensajes accionables
- Reconocer en vez de recordar: etiquetas visibles, no solo placeholder

Note:
La usabilidad mide eficacia, eficiencia y satisfacción, y las diez heurísticas son el referente clásico. De ellas, varias se ejecutan directamente en CSS: estados de botón, mensajes de validación y targets táctiles generosos. Fijaos en la sexta: el placeholder nunca sustituye a la etiqueta.

---

## Diagrama · flujo de verificación

```text
Teclado   Tab por toda la pagina  ->  foco visible y orden logico
   |
   v
Zoom      200 % y ancho 320 px    ->  sin cortes ni scroll doble
   |
   v
Automat.  axe o Lighthouse        ->  detecta 30-40 % de los fallos
   |
   v
Manual    lector de pantalla      ->  la parte que nadie automatiza
```

- Primero lo manual con teclado, que es rápido y descubre lo grave
- Después la auditoría automática para cubrir el criterio verificable
- La revisión con personas usuarias es lo que ninguna herramienta da
- En proyectos con RTL, repetid el recorrido con el idioma alternativo

Note:
El orden importa: la automatización al final sirve para confirmar, no para descubrir. Que las herramientas solo cubran un tercio de los problemas es el dato que más sorprende y el que justifica el trabajo manual. En el RA 5 se pide explícitamente verificar con test externos.

---

## Herramientas de verificación

<span class="fragment">axe DevTools: la auditoría más completa e integrable en CI</span>

<span class="fragment">Lighthouse: accesibilidad, rendimiento y SEO en un mismo informe</span>

<span class="fragment">WAVE visualiza los errores superpuestos sobre la propia página</span>

<span class="fragment">Pa11y sirve para pipelines desde línea de comandos</span>

<span class="fragment">WebAIM Contrast Checker para ratios rápidos de dos colores</span>

Note:
Conviene tener dos de estas herramientas en la rutina, una en el navegador y una en la integración continua. El contraste se mide con cualquiera de las de color, y el resultado es un número que se puede documentar. Ninguna sustituye al recorrido con teclado y lector de pantalla.

---

## Error común: accesibilidad rota en CSS

⚠ `outline: none` global sin foco visible (2.4.7)

⚠ Grises sobre blanco: el criterio 1.4.3 más suspende

⚠ Placeholder como etiqueta desaparece al escribir

⚠ `fixed` tapa el primer campo al pulsar Tab (2.4.11)

⚠ Animación infinita sin `reduced-motion` (2.3.3)

⚠ Menú que se cierra al acercar el ratón (1.4.13)

Note:
Los tres primeros son los que aparecen en casi toda auditoría de un proyecto estudiante. El del foco tapado pasa desapercibido porque solo se ve con teclado. Los dos últimos son de interacción y se descubren probando, no leyendo código. Conocer el número del criterio suma en el examen.

---

## Autoevaluación

<span class="fragment">1. ¿Qué ratio exige 1.4.3 para texto normal y cuándo baja a 3:1?</span>

<span class="fragment">2. ¿Es accesible un botón con `opacity: 0`? Justifica la respuesta</span>

<span class="fragment">3. Tu header fijado tapa el primer campo al pulsar Tab. ¿Qué CSS aplicas?</span>

--

**1)** 4.5:1 con texto normal; baja a 3:1 cuando el texto es grande (24 px o 18.66 en negrita).

**2)** No: sigue en el árbol y es enfocable, así que se activa algo que no se ve.

**3)** `scroll-margin-top` en los elementos con id o con `:target`, por encima del header.

Note:
La primera es la cifra que hay que tener grabada. La segunda es la pregunta trampa de la tabla de ocultación: lo accesible no es lo mismo que lo invisible. La tercera conecta el criterio 2.4.11 con la solución concreta de CSS, que es lo que se pide en la práctica.

---

## Claves para el examen

- POUR: perceptible, operable, comprensible y robusto; nivel objetivo AA
- Contraste de texto 4.5:1 y de componentes 3:1; el color nunca va solo
- `outline: none` solo con sustituto estilado en `:focus-visible`
- `display: none` y `opacity: 0` no son lo mismo: este último sigue enfocable
- `.visually-hidden` deja el contenido legible para el lector de pantalla
- Las herramientas automáticas cubren 30–40 %: el resto es revisión manual

Note:
Seis líneas para repasar la noche anterior. Las tres primeras son de criterios WCAG con sus cifras, las dos siguientes de técnica y la última de proceso. Si recordáis la tabla de ocultación y el flujo del diagrama, la unidad está aprobada.
