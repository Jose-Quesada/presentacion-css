# CSS

## Unidad 9 · Fondos, imágenes y decoración

**Módulos 0615 (DIW) · 0488 (DI)**

Diseño de interfaces web (DAW) · Desarrollo de Interfaces (DAM)

Curso 2026/2027

Note:
Bienvenidos a la unidad de fondos, imágenes y decoración. Cubrimos el RA 3 y el RA 4 desde el lado CSS: cómo presentar, optimizar y decorar con imágenes. Es la unidad donde se decide el peso real de una página y su accesibilidad visual. Pregunta de arranque: ¿cuánto pesa la imagen más grande de vuestro último proyecto?

---

## Objetivos de aprendizaje

<span class="fragment">1. Dominar la propiedad `background` y sus <mark>ocho subpropiedades</mark></span>

<span class="fragment">2. Generar <mark>degradados</mark> y patrones sin descargar ninguna imagen</span>

<span class="fragment">3. Elegir <mark>formatos de imagen</mark> y servirlos con srcset y picture</span>

<span class="fragment">4. Distinguir recorte clip-path, máscara mask y <mark>outline de foco</mark></span>

Note:
Cuatro objetivos que van del fondo a la imagen y de la imagen a la decoración. El tercero es el de rendimiento y el cuarto, el de accesibilidad. Los dos primeros parecen estéticos, pero condicionan cuántas peticiones y cuánto peso arrastra la página. Practicad cada bloque en un ejercicio propio.

---

## Motivación · El peso lo decide la imagen

<div style="font-size: 1em; text-align: left;">

El RA 3 y el RA 4 piden preparar e integrar contenido multimedia con criterio
</div>

<span class="fragment">Una imagen de 200 KB mal elegida pesa más que toda la hoja CSS</span>

<span class="fragment">Sin `width` y `height` la imagen provoca saltos de diseño al cargar</span>

<span class="fragment">Las fotos decorativas deberían ser fondo; las informativas, `<img>`</span>

<span class="fragment">Cada petición y cada kilobyte cuentan en la métrica de carga</span>

Note:
La mayoría de páginas lentas lo son por multimedia, no por CSS. Optimizar imagen es la palanca de rendimiento con mejor relación esfuerzo y resultado. Además, las decisiones de aquí se auditan: peso, desplazamiento de diseño y accesibilidad de la imagen. Empezad siempre por lo que más pesa.

---

## Motivación · Métricas que se pueden medir

<div style="font-size: 1em; text-align: left;">

Tres indicadores que mejoran al aplicar bien esta unidad
</div>

<span class="fragment">LCP: la imagen principal carga grande, temprano y sin GIF pesados</span>

<span class="fragment">CLS: dimensiones declaradas, cero saltos al aparecer el contenido</span>

<span class="fragment">Peso total: formatos actuales y variantes según el viewport</span>

<span class="fragment">Accesibilidad: `alt` descriptivo y foco visible en todo el recorrido</span>

Note:
Las métricas convierten opiniones en números comparables antes y después del cambio. Con Lighthouse se comprueban en un minuto en modo móvil. El desplazamiento de diseño es el que más molesta al usuario, porque mueve el texto bajo el dedo. Todas estas métricas entran en la evaluación del módulo.

---

## background · la propiedad maestra

- `background` es un <mark>atajo</mark>: lo que no escribas se resetea
- Acepta listas de capas y la primera se pinta encima
- `cover` llena recortando; `contain` deja que la imagen quepa entera
- `background-clip: text` recorta el degradado al propio texto
- `background-attachment: fixed` da parallax barato, costoso en móvil

Note:
Detrás del atajo hay ocho subpropiedades, cada una con lista de capas. Las listas se emparejan por posición: tres tamaños para tres capas. La primera de la lista se pinta encima, que es el detalle que más se olvida al montar fondos compuestos. El orden libre del atajo tiene una excepción: la primera longitud es posición y la segunda, tamaño.

---

## Degradados · linear-gradient es el rey

```css
background-image: linear-gradient(
  135deg,                   /* dirección: 0deg arriba, 90deg derecha */
  var(--c1) 0%,              /* parada inicial */
  var(--c2) 50%, var(--c3) 100%
);
```

- `radial-gradient` añade forma y origen: `circle at top left`
- `conic-gradient` sirve para donuts, aros y medidores
- `repeating-linear-gradient` pinta rayas con cero peticiones

Note:
Un degradado es dirección más paradas de color, y se genera en el cliente sin peticiones. Las paradas dobles crean tramos sólidos, útil para banderas o divisiones. Interpolando en `oklab` se evita el feo paso por marrón entre colores opuestos. Los degradados también sirven de fondo de carga para imágenes.

---

## Formatos de imagen · la regla de oro

| Formato | Tipo | Uso |
|---|---|---|
| AVIF / WebP | Raster | Fotos: mejor compresión |
| JPEG / PNG | Raster | Herencia: fotos y gráficos |
| SVG | Vector | Iconos y logos geométricos |

<span class="fragment">Foto → <mark>AVIF o WebP</mark>; icono o logo → <mark>SVG</mark></span>

<span class="fragment">Calidad 70–85 y tamaño real; el GIF pasa a WebP animado</span>

Note:
AVIF comprime entre un 20 y un 50 % menos que WebP con la misma calidad. El PNG pesa, pero salva los casos de transparencia compleja que el JPEG no admite. Recordad optimizar el SVG con SVGO e inlinearlo si el icono es pequeño. Servir imágenes más grandes de lo necesario es el error más caro.

---

## srcset y sizes · el navegador elige

```html
<img src="https://dummyimage.com/800x600/ccc/000.png&text=foto-800.webp"
     srcset="https://dummyimage.com/400x300/ccc/000.png&text=foto-400.webp 400w, https://dummyimage.com/800x600/ccc/000.png&text=foto-800.webp 800w, https://dummyimage.com/800x600/ccc/000.png&text=foto-1600.webp 1600w"
     sizes="(max-width: 600px) 100vw, 50vw"
     alt="Descripción de la foto"
     width="800" height="600" loading="lazy">
```

- Los sufijos `400w` declaran el ancho real de cada variante
- `width` y `height` reservan espacio y evitan el salto de diseño
- `<picture>` cambia el corte entre móvil y escritorio con `media`

Note:
`srcset` con `sizes` es la forma nativa de servir el tamaño correcto según el viewport. `width` y `height` no dimensionan la imagen, solo dan la proporción al navegador. Para la imagen principal, además, se usa `fetchpriority` en lugar de lazy. La regla `sizes` describe el hueco real que ocupará la imagen.

---

## object-fit y aspect-ratio

```css
.card img {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;      /* fill | contain | cover | none */
  object-position: center top;
}
```

- `cover` recorta por los bordes manteniendo la proporción
- `contain` muestra la imagen entera y deja bandas a los lados
- Con `aspect-ratio` las galerías quedan uniformes

Note:
`object-fit` resuelve el recorte elegante sin retocar el archivo. `object-position` decide qué zona se conserva, muy útil con retratos. La combinación con `aspect-ratio` sustituye al viejo truco del padding porcentual. En tarjetas de listado, casi siempre se usa cover centrado arriba.

---

## Imagen de fondo o imagen de contenido

<span class="fragment">Decorativa → `background-image`: invisible para lectores de pantalla</span>

<span class="fragment">Con información → `<img>` con alt, seleccionable y enfocable</span>

<span class="fragment">El fondo no ocupa caja propia; el `<img>` sí reserva espacio</span>

<span class="fragment">Solo el `<img>` permite variantes con `srcset` y `sizes`</span>

<span class="fragment">El fondo puede no cargarse si el elemento no llega a pintarse</span>

Note:
Esta es la pregunta de examen recurrente de la unidad: decorativa o informativa. Si la imagen comunica algo, tiene que ser `<img>` con `alt`; si adorna, fondo. Las imágenes de fondo además pueden no cargarse si el elemento no se pinta. También afecta a la selección de texto y al copiar y pegar.

---

## Bordes, sombras y contorno de foco

```css
.tarjeta { border: 1px solid var(--borde); border-radius: 1rem; }
.elevada { box-shadow: 0 1px 2px rgb(0 0 0 / .08),
                       0 8px 24px rgb(0 0 0 / .12); }
:focus-visible { outline: 2px solid var(--enfoque); outline-offset: 2px; }
```

- Dos capas de sombra: contacto corta y elevación difusa
- Las sombras no ocupan layout: no empujan a los vecinos
- `outline` no ocupa espacio y nunca se elimina sin reemplazo

Note:
El borde necesita siempre estilo: sin `border-style` no se dibuja. La sombra por capas da un aspecto de material mucho más creíble que una sola capa difusa. Lo crítico es el `outline` de foco: quitarlo sin sustituto rompe la navegación por teclado. El radio mayor que la mitad del lado produce una elipse automática.

---

## Filtros, vidrio y modos de mezcla

<span class="fragment">`filter` altera los píxeles ya pintados del propio elemento</span>

<span class="fragment">`drop-shadow()` respeta la alfa; `box-shadow` sigue la caja rectangular</span>

<span class="fragment">`backdrop-filter` da el efecto vidrio con coste alto de GPU</span>

<span class="fragment">`mix-blend-mode: multiply` oscurece y `screen` aclara</span>

<span class="fragment">`background-blend-mode` mezcla las capas del fondo entre sí</span>

Note:
La tríada clave: `filter` modifica el elemento, `drop-shadow` añade sombra respetando formas irregulares y `mix-blend-mode` lo mezcla con lo que hay detrás. El vidrio es bonito pero caro: conviene limitarlo a unos pocos elementos fijos. En listas largas, el filtro de fondo hunde los frames por segundo.

---

## clip-path frente a mask

<span class="fragment"><mark>clip-path</mark> recorta en duro: dentro o fuera, sin grises</span>

<span class="fragment">`mask-image` trabaja por alfa y admite medios tonos</span>

<span class="fragment">Formas geométricas → `clip-path: circle, polygon, inset`</span>

<span class="fragment">Fundidos y desvanecidos → `mask-image` con un degradado</span>

<span class="fragment">La máscara admite composición y modo de luminancia o alfa</span>

Note:
Ambas parecen iguales y responden a necesidades distintas: recortar una forma o difuminar un borde. `clip-path` no afecta al contenido que queda fuera del recorte y también recorta las sombras. La máscara, en cambio, permite transiciones suaves. Con `path()` se puede recortar con un trazado SVG libre.

---

## Ejemplo · degradados sin descargar nada

```css
/* Texto con degradado: el fondo se recorta en las letras */
.titulo {
  background: linear-gradient(90deg, #6366f1, #ec4899);
  background-clip: text;
  color: transparent;
}
/* Rayas generadas por el navegador, cero peticiones */
.via { background: repeating-linear-gradient(90deg, #facc15 0 40px, transparent 40px 80px); }
```

<span class="fragment">Patrón generado en el cliente y escala infinita sin pixelar</span>

<span class="fragment">Con `color: transparent` el texto sigue siendo seleccionable</span>

<span class="fragment">Un borde degradado se logra con doble fondo: padding y border</span>

Note:
El truco del texto degradado es dos líneas: fondo recortado al texto y color transparente. En Safari antiguo hacía falta el prefijo webkit de background-clip. Las rayas de repetición valen para cualquier patrón de bandas sin servir un PNG. El doble fondo del borde es el otro clásico de examen.

---

## Ejemplo · skeleton loader animado

```css
.skeleton {
  background: linear-gradient(90deg, #eee 25%, #f7f7f7 50%, #eee 75%);
  background-size: 200% 100%;      /* más ancho que la caja */
  animation: shimmer 1.2s infinite;
}
@keyframes shimmer { to { background-position: -200% 0; } }
```

<span class="fragment">Simula la espera de la carga sin bloquear la interfaz</span>

<span class="fragment">Combínalo con `prefers-reduced-motion` para no marear (unidad 10)</span>

<span class="fragment">Reserva el espacio final, así que no produce saltos de diseño</span>

Note:
El skeleton necesita un degradado más ancho que la caja y desplazar el fondo en bucle. Es más honesto que un spinner porque reserva el espacio final y evita saltos. Recuerda respetar la preferencia de movimiento reducido al animarlo. El mismo truco sirve para brillos sobre tarjetas en carga.

---

## Diagrama · ¿Fondo o etiqueta img?

<div style="font-size: 1em; text-align: left;">

¿La imagen aporta información? → SÍ → `<img>` con `alt` y dimensiones<br>
&nbsp;&nbsp;&nbsp;&nbsp;→ NO → `background-image`, decoración pura<br>
¿Es foto o icono? → foto → AVIF o WebP con `srcset` · icono → SVG inline<br>
¿Es el elemento principal? → entonces sin `loading="lazy"`

</div>

Note:
Es el árbol de decisión que conviene repetir en cada componente con imagen. Primero se decide la semántica, después el formato y por último el modo de servirlo. Si invertís el orden acabáis con fotos de contenido metidas en fondos. El último paso evita retrasar el elemento más grande de la página.

---

## Ejemplo práctico · Hero banner con degradados, glassmorphism y clip-path

```css
.hero-banner {
  /* Fondo multicapa: iluminación radial sobre degradado angular */
  background:
    radial-gradient(circle at 80% 20%, rgb(99 102 241 / .4), transparent 50%),
    linear-gradient(135deg, #0f172a 0%, #1e1b4b 100%);
  clip-path: polygon(0 0, 100% 0, 100% 90%, 0 100%); /* Recorte diagonal */
  padding: 4rem 2rem;
}

.tarjeta-cristal {
  background: rgb(255 255 255 / .12);
  backdrop-filter: blur(12px); /* Desenfoque de lo que queda detrás */
  border: 1px solid rgb(255 255 255 / .25);
  border-radius: 12px;
}

@media (prefers-reduced-transparency: reduce) {
  .tarjeta-cristal {
    backdrop-filter: none;
    background: #1e293b; /* Opaco accesible para legibilidad */
  }
}
```

<span class="fragment">Degradados múltiples en una sola declaración para iluminaciones complejas</span>

<span class="fragment"><code>backdrop-filter: blur()</code> genera estética glassmorphism en tiempo real</span>

<span class="fragment"><code>clip-path: polygon()</code> recorta siluetas geométricas aceleradas por GPU</span>

<span class="fragment">Accesibilidad: <code>prefers-reduced-transparency</code> aporta fondo opaco a quien lo requiera</span>

Note:
Técnicas de decoración y estilado gráfico avanzado:
1. Composición de fondos: background admite múltiples capas separadas por comas. El radial-gradient simula un foco de luz dinámico que se funde con el fondo oscuro general.
2. backdrop-filter: desenfoca y satura exclusivamente el contenido que está físicamente por debajo de la tarjeta, creando un acabado visual sofisticado sin recurrir a Photoshop ni assets pesados.
3. clip-path: polygon() recorta la caja sin afectar a las sombras proyectadas ni al flujo de lectura de los hijos.
4. Consulta de medios prefers-reduced-transparency: vital para personas con déficit de contraste o problemas neurológicos, sustituyendo la transparencia por un color sólido y nítido.

---

## Error común: imagen de contenido en background

⚠ Foto en `background-image`: no admite <mark>`alt`</mark>

⚠ Sin `width`/`height` hay salto de diseño al cargar

⚠ `cover` sin `background-position` recorta donde no deba

⚠ GIF de 3 MB para animar: métrica de carga desastrosa

⚠ `outline: none` sin foco visible (WCAG 2.4.7)

Note:
El primero es el error grave de accesibilidad: la información queda fuera del lector de pantalla. El segundo se audita con la métrica de desplazamiento de diseño. Los últimos tres son de rendimiento y usabilidad, igual de fáciles de evitar. Revisad vuestras plantillas con esta lista antes de entregar.

---

## Autoevaluación

<span class="fragment">1. ¿Qué hace `object-fit: cover` con `aspect-ratio: 1` en una foto 16:9?</span>

<span class="fragment">2. Diferencia `clip-path` y `mask-image` con un ejemplo de cada uno</span>

<span class="fragment">3. Justifica el formato para: logo, foto de producto y animación de 2 s</span>

--

**1)** Recorta por los lados hasta llenar el cuadrado manteniendo la proporción.

**2)** `clip-path` recorta en duro; `mask-image` funde con alfa hacia el vacío.

**3)** Logo → SVG; foto de producto → AVIF o WebP; animación → WebP animado.

Note:
Las tres preguntas cubren recorte, máscara y elección de formato. La tercera es la del RA 3 y admite discusión, pero la justificación debe citar peso, transparencia y soporte. Si falláis la primera, repasad la diapositiva de object-fit. La segunda conecta con la decoración de la unidad siguiente.

---

## Claves para el examen

- `background` es un atajo: lo no indicado se resetea y la primera capa va encima
- Foto → AVIF o WebP; icono → SVG; calidad 70–85 y dimensiones reales
- Decorativa → `background`; con información → `<img>` con `alt`
- `width`, `height` y `aspect-ratio` declarados: cero saltos de diseño
- `clip-path` recorta en duro; `mask` trabaja con alfa
- `outline` nunca se elimina sin un reemplazo visible

Note:
Seis ideas que resumen la unidad entera. Las dos primeras son de optimización, las dos centrales de integración y las dos últimas de decoración y foco. El patrón a recordar es: primero la semántica, después la estética. Practicad el árbol de decisión del diagrama con tres imágenes reales.
