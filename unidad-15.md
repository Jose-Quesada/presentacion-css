# CSS

## Unidad 15 · Ejercicios, glosario y recursos

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de repaso. Aquí se concentra el banco de ejercicios por niveles, el glosario español-inglés y las fuentes oficiales para seguir estudiando. Es la unidad que se usa la noche antes del examen y la semana del proyecto. Pregunta de arranque: ¿por dónde empezaríais a repasar: selectores, layout o accesibilidad?

---

## Objetivos de aprendizaje

<span class="fragment">1. Resolver los ejercicios <mark>sin mirar</mark> la solución y corregirlos después</span>

<span class="fragment">2. Razonar <mark>especificidad</mark>, colapso de márgenes y capas de z-index</span>

<span class="fragment">3. Justificar cada auditoría citando el <mark>criterio WCAG</mark> que falla</span>

<span class="fragment">4. Entregar el proyecto con <mark>capas, tokens</mark> y decisiones documentadas</span>

<span class="fragment">5. Domina el glosario <mark>ES/EN</mark> de la documentación técnica</span>

Note:
Cinco objetivos de aplicación: prácticamente nada de memorización. El primero es de método y es el que más diferencia la nota. El quinto parece menor, pero es lo que os permite leer MDN y las especificaciones sin traducir cada término.

---

## Motivación · cómo usar este banco

<div style="font-size: 1em; text-align: left;">

Primero el enunciado en papel o en código; la solución solo para corregir
</div>

<span class="fragment" style="font-size: 1em;">Cada ejercicio está marcado con la unidad que evalúa</span>

<span class="fragment" style="font-size: 1em;">Nivel medio para consolidar, nivel avanzado para el examen de práctica</span>

<span class="fragment" style="font-size: 1em;">Copiar la solución sin rehacer el ejercicio no sirve de nada</span>

<span class="fragment" style="font-size: 1em;">Annotad el fallo de cada corrección: ahí está vuestro repaso personalizado</span>

Note:
El error metodológico clásico es leer la solución y sentir que se ha entendido. Entender no es lo mismo que saber hacerlo en veinte minutos con el en blanco. Trabajad por bloques de tres o cuatro ejercicios y revisad solo al terminar el bloque.

---

## Nivel medio · bloques E1 a E3

<span class="fragment">E1: calcular la especificidad de cinco selectores y ordenarlos</span>

<span class="fragment">E2: sumar caja con `content-box` y con `border-box` y con márgenes de hermano</span>

<span class="fragment">E3: explicar por qué un modal `fixed` se desplaza con un ancestro animado</span>

<span class="fragment">Clave de E1: `:where()` pesa cero y `:is()` toma su argumento mayor</span>

<span class="fragment">Clave de E3: el `transform` crea bloque contenedor del posicionamiento</span>

Note:
Los tres primeros ejercicios tocan las unidades de fundamentos, caja y posicionamiento, que son las más preguntadas. En E1 el truco está en sumar el argumento de los selectores funcionales, no el nombre de la función. En E3 hay que citar la consecuencia: el fixed deja de referirse al viewport.

---

## Nivel medio · bloques E4 a E7

<span class="fragment">E4: navbar con logo a la izquierda, enlaces a la derecha y envoltura en móvil</span>

<span class="fragment">E5: galería de 4, 2 y 1 columnas <mark>sin media queries</mark></span>

<span class="fragment">E6: escala tipográfica fluida con `clamp()` calculada a mano</span>

<span class="fragment">E7: widget que cambia de disposición con container queries</span>

<span class="fragment">Clave de E5: `auto-fit` colapsa huecos y `auto-fill` los reserva</span>

Note:
Estos cuatro son de layout y responden casi siempre con una sola propiedad bien elegida. En E6 lo importante es el método de interpolación lineal entre los dos puntos de diseño, no la calculadora. En E7 se comprueba que sabéis declarar el contenedor antes de la consulta.

---

## Ejemplo · E5, galería fluida sin consultas

```css
.galeria {
  display: grid;
  grid-template-columns:
    repeat(auto-fit, minmax(min(220px, 100%), 1fr));
  gap: 1rem;
}
.galeria > img {
  width: 100%;
  aspect-ratio: 4 / 3;
}
```

- `auto-fit` colapsa las columnas vacías y estira las ocupadas
- `min(220px, 100%)` evita el desborde en contenedores más estrechos
- Con `aspect-ratio` la galería no salta al cargar las imágenes

Note:
Esta solución resuelve el enunciado completo con seis líneas y sin ninguna media query. El `min()` interior es el detalle que separa una solución que funciona de otra que desborda en móvil. Comprobadlo redimensionando la ventana hasta el extremo.

---

## Nivel avanzado · bloques E8 a E10

<span class="fragment">E8: acordeón accesible con `details`, `summary` y `:has()` <mark>sin JavaScript</mark></span>

<span class="fragment">E9: sistema de capas de `z-index` documentado con tokens</span>

<span class="fragment">E10: barra de progreso de lectura con scroll-driven y su fallback</span>

<span class="fragment">Clave de E8: `grid-template-rows` de `0fr` a `1fr` sí se puede transicionar</span>

<span class="fragment">Clave de E9: `isolation: isolate` aísla la escala local de un componente</span>

Note:
Los tres primeros avanzados combinan dos unidades cada uno, que es exactamente como llegan en el examen. El truco del acordeón evita el clásico salto de altura con overflow oculto. Y con las capas, la recomendación es siempre subir el token, nunca el número mágico.

---

## Ejemplo · E8, acordeón sin JavaScript

```css
.acordeon summary { cursor: pointer; padding: 1rem; }
.acordeon summary::marker { content: "+ "; }
.acordeon[open] summary::marker { content: "- "; }
.acordeon .cuerpo {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows .3s ease-out;
}
.acordeon[open] .cuerpo { grid-template-rows: 1fr; }
.acordeon .cuerpo > * { overflow: hidden; }
```

- `<details>` ya gestiona estado, teclado y semántica sin una línea de JS
- La animación usa la transición de `grid-template-rows`, que sí interpola
- `:has()` permite atenuar el resto del grupo cuando alguno está abierto

Note:
Este fragmento es la respuesta completa del E8 y resume media asignatura: selectores funcionales, transiciones y accesibilidad. Fijaos en que el overflow está en el hijo, no en la caja animada, porque si no la animación no se ve. Practicad ampliándolo a varios grupos de acordeón.

---

## Nivel avanzado · bloques E11 a E13

<span class="fragment">E11: migrar CSS heredado a <mark>capas, tokens, anidación y lógicas</mark></span>

<span class="fragment">E12: auditar un fragmento listando cada fallo con su criterio WCAG</span>

<span class="fragment">E13: plan de mejora para un LCP de 4.8 segundos en móvil</span>

<span class="fragment">Clave de E11: el modificador mal nombrado pasa a BEM con token de color</span>

<span class="fragment">Clave de E13: se ataca primero la imagen LCP y después el CSS</span>

Note:
E11 es el ejercicio que más se parece al trabajo real del módulo: ordenar lo heredado sin cambiar el resultado visual. E12 exige citar el número de criterio, no solo decir «falta contraste». E13 se corrige por orden de impacto, que es como se justifica ante un cliente.

---

## Proyecto integrador · entrega mínima

```text
/landing
  ├─ index.html          semántica, contenido y foco real
  ├─ css/tokens.css      capas, variables y modo oscuro
  ├─ css/componentes.css nav, hero, tarjetas y footer
  └─ README.md           decisiones de a11y y rendimiento
```

- Header fijo con nav en flex que colapsa en móvil
- Hero con imagen responsiva, título fluido con `clamp()` y llamada a la acción
- Características en grid `auto-fit` de 3 a 1 columnas
- Tarjetas con hover animado y estados de foco visibles
- Barra de lectura con scroll-driven y degradación elegante

Note:
Lo que se valora es cómo está organizado y justificado, no cuántos archivos tenga. Esta estructura es la mínima aceptable y ya demuestra capas, tokens y separación de responsabilidades. El README es parte de la entrega: ahí se explica el porqué de cada decisión.

---

## Rúbrica de corrección

- Estructura semántica del HTML: 20 %
- Calidad del CSS con capas, tokens y nomenclatura: 25 %
- Responsive real, probado en varios anchos: 20 %
- Accesibilidad verificada con recorrido de teclado: 20 %
- Rendimiento: peso, métricas y fuentes: 15 %

- Antes de empezar: E1 a E7 sin mirar soluciones y E11 rehecho desde cero

Note:
El reparto es el que se propone para la corrección tipo examen práctico del módulo 0615. Fijaos en que la calidad del CSS pesa más que el responsive: se paga la organización. El último bullet es el checklist de preparación previa, y es la parte que depende de vosotros.

---

## Glosario · cascada y caja

- Cascada <span>Cascade</span>: resolución de conflictos entre declaraciones
- Especificidad <span>Specificity</span>: peso relativo de un selector
- Colapso de márgenes <span>Margin collapsing</span>: fusión de márgenes verticales
- Bloque contenedor <span>Containing block</span>: referencia del `absolute`
- Contexto de apilamiento <span>Stacking context</span>: capa aislada para `z-index`

Note:
Son los términos que aparecen en casi todos los exámenes, junto con el contexto de formato de bloque. Aprended el par español-inglés de dos en dos y ancladlo a un ejemplo de código. La columna en inglés es la que veréis en MDN y en las ofertas de trabajo.

---

## Glosario · layout y consultas

- Pista del grid <span>Track</span>: fila o columna de la cuadrícula
- Línea del grid <span>Grid line</span>: borde de una pista
- Subcuadrícula <span>Subgrid</span>: herencia de pistas del grid padre
- Eje principal <span>Main axis</span>: dirección del flujo en flexbox
- Consulta de contenedor <span>Container query</span>: condición sobre el hueco
- Propiedad personalizada <span>Custom property</span>: variable CSS con `--`

Note:
Aquí los pares gemelos se confunden con facilidad, sobre todo pista y línea, que no significan lo mismo. El subgrid es el término más reciente y por eso se pregunta más. Conviene poder escribir en inglés el nombre de cada concepto, porque así se busca en la documentación.

---

## Glosario · movimiento, rendimiento y a11y

- Transición <span>Transition</span>: interpolación entre dos estados
- Fotogramas clave <span>Keyframes</span>: estados intermedios de una animación
- Promoción a capa <span>Layer promotion</span>: elevar a compositor con `will-change`
- Renderizador bloqueante <span>Render-blocking</span>: retrasa la primera pintura
- Repintado <span>Repaint</span>: redibujar sin recalcular geometría
- Contraste <span>Contrast ratio</span>: relación de luminancias de texto y fondo

Note:
Este bloque cubre las tres últimas unidades, que son las que más preguntas nuevas generan. Ojo con el par repaint y reflow: uno recalcula geometría y el otro solo pinta. Del lado de accesibilidad conviene repetir también movimiento reducido y colores forzados.

---

## Términos que se confunden

| Pareja | Diferencia |
|---|---|
| Cascade / specificity | Conflicto frente a peso |
| Track / line | Pista frente a borde |
| Repaint / reflow | Repintar frente a recalcular |

- Las tres parejas que más se mezclan en examen
- Track/line es de grid; repaint/reflow, del coste por fotograma
- Se entienden mejor con un ejemplo que con la definición

Note:
Cuando una pregunta de examen enfrenta dos términos parecidos, casi siempre es una de estas tres parejas. Practicad a explicar la diferencia en una frase propia, sin citar la definición. Si sabéis decir qué cambia en la práctica, tenéis la pregunta resuelta.

---

## Recursos · documentación y especificaciones

- MDN en español: la referencia diaria para dudas y compatibilidad
- caniuse antes de estrenar cualquier API nueva en producción
- web.dev y Chrome Developers: rendimiento y Core Web Vitals
- Specs del W3C: Selectors, Color, Grid, Cascade y Animations Level 2
- La lista de especificaciones está en los apuntes, sin necesidad de leerlas todas

Note:
La regla de uso es práctica: no se leen las especificaciones de corrido, se entra a buscar un criterio concreto. MDN resuelve el 90 % de las dudas diarias y caniuse os salva del susto de compatibilidad. Guardad ambos en marcadores desde hoy.

---

## Recursos · accesibilidad, práctica y normativa

- WCAG 2.2 con sus técnicas: el criterio y cómo cumplirlo
- WebAIM y The A11Y Project: artículos y verificador de contraste
- axe DevTools y Pa11y para auditar en el navegador y en el pipeline
- CSS Battle, Smol CSS y Every Layout para practicar con retos
- Orden de 2011 del currículo de DAW y RD 686/2010 del título

Note:
El bloque de accesibilidad es el que más os servirá en el proyecto y en el primer trabajo. Los retos cortos sirven para afianzar sintaxis sin montar un proyecto entero. Y la normativa está ahí porque el módulo se implica dentro de un currículo oficial que conviene conocer.

---

## Error común: estudiar de memoria

⚠ Copiar sin rehacer el ejercicio: en el examen no estará

⚠ `:is()` y `:not()` toman el peso de su argumento

⚠ `:where()` pesa cero; `:is()` pesa su argumento

⚠ Sumar márgenes que colapsan: vale el mayor

⚠ Auditar sin citar el número de criterio WCAG

Note:
Los tres primeros son errores de concepto y se arreglan repasando la diapositiva de especificidad. El cuarto es el clásico del modelo de caja. El último hace perder puntos enteros en práctico: sin el criterio, la respuesta es una opinión.

---

## Autoevaluación

<span class="fragment">1. Ordena de menor a mayor: `:where(.a) > span`, `.card .t`, `#nav li a`</span>

<span class="fragment">2. Dos hermanos con `margin: 20px 0`: ¿qué espacio queda entre ellos?</span>

<span class="fragment">3. Cita el criterio WCAG que falla con `outline: none` en el foco</span>

--

**1)** `:where(.a) > span` con peso 0-0-1, después la clase doble y al final el ID.

**2)** Quedan 20 px: los márgenes verticales colapsan y gana el mayor.

**3)** Criterio 2.4.7 de nivel AA, foco visible; se arregla con `:focus-visible`.

Note:
Las tres preguntas son las más repetidas del banco, y las tres se resuelven en menos de un minuto si están repasadas. La primera comprueba la especificidad moderna, la segunda el modelo de caja y la tercera la accesibilidad. Si os falla alguna, volved al ejercicio original correspondiente.

---

## Claves para el examen

- Primero el enunciado en papel; la solución solo para corregir al final
- `:where()` pesa cero y `:is()` o `:not()` pesan su argumento
- Los márgenes verticales colapsan: entre hermanos gana el mayor
- `auto-fit` colapsa los huecos vacíos y `auto-fill` los reserva
- Las capas se aíslan con tokens de `z-index` y con `isolation: isolate`
- Toda auditoría se contesta citando el criterio WCAG que falla

Note:
Seis ideas para la noche anterior al examen. Las tres primeras son de fundamentos, la cuarta de grid y las dos últimas de arquitectura y calidad. Del proyecto se valora estructura, justificación y verificación: no hace falta mucho CSS, hace falta CSS que se pueda defender.
