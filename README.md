# Orwelliano Eyes

Blog personal sobre tecnología, política y govtech. Un solo archivo HTML, sin
servidor, sin base de datos y sin dependencias más allá de dos tipografías.

---

## 1. Publicarlo en GitHub Pages

No hace falta instalar nada ni usar la terminal. Todo se hace desde el navegador.

### Crear el repositorio

1. Entrá a [github.com](https://github.com) e iniciá sesión.
2. Botón **+** arriba a la derecha → **New repository**.
3. En **Repository name** escribí exactamente:

   ```
   tuusuario.github.io
   ```

   Reemplazando `tuusuario` por tu nombre de usuario de GitHub. Esto importa:
   si el repositorio se llama así, el blog queda en `https://tuusuario.github.io`.
   Si le pones cualquier otro nombre, queda en
   `https://tuusuario.github.io/nombre-del-repo/`, que es más largo y menos
   prolijo para compartir.
4. Marcá **Public**. Tiene que ser público para que Pages funcione en la cuenta
   gratuita.
5. **Create repository**.

### Subir el archivo

1. En el repositorio vacío, clic en **uploading an existing file**.
2. Arrastrá `index.html`. El nombre tiene que ser exactamente ese, en minúscula:
   es el archivo que los servidores web sirven por defecto.
3. Abajo, en el cuadro de mensaje, escribí algo como `primera versión`.
4. **Commit changes**.

### Activar Pages

1. Pestaña **Settings** del repositorio.
2. Menú lateral izquierdo → **Pages**.
3. En **Source** elegí **Deploy from a branch**.
4. En **Branch** elegí `main` y carpeta `/ (root)`. **Save**.
5. Esperá entre uno y tres minutos. Recargá esa misma página de Settings y va a
   aparecer arriba el link de tu sitio.

La primera vez tarda. Cada actualización posterior tarda menos de un minuto.

### Publicar una nota nueva, de ahí en adelante

1. Editás `index.html` en tu computadora.
2. En el repositorio, clic en `index.html` → ícono del lápiz (**Edit this file**).
3. Borrás todo el contenido, pegás la versión nueva, **Commit changes**.

Alternativa más rápida: arrastrar el archivo con **Add file → Upload files**.
GitHub lo reemplaza y guarda la versión anterior en el historial.

> **Ventaja que conviene conocer:** GitHub guarda cada versión que subís. Si
> borrás algo por error, entrás a la pestaña **Commits** y recuperás cualquier
> estado anterior del blog. Es un respaldo automático que no tenés que
> administrar.

### Dominio propio (opcional)

Si en algún momento compras un dominio (`orwellianoeyes.com`, por ejemplo), se
conecta desde **Settings → Pages → Custom domain**. GitHub sigue alojando el
sitio gratis y suma el certificado HTTPS solo.

---

## 2. Cómo escribir una nota

Dentro de `index.html`, buscá el bloque de comentarios que dice
`ACÁ ESCRIBÍS LOS ARTÍCULOS`. Copiás un `<article>` completo, lo pegás en
cualquier lugar de esa zona y cambiás cuatro cosas.

| Qué | Dónde | Valores |
|---|---|---|
| Categoría | `data-cat` | `tecnologia`, `politica` o `govtech` — sin acento, en minúscula |
| Fecha | `data-fecha` | `AAAA-MM-DD`, por ejemplo `2026-08-14` |
| Identificador | `id` | Texto corto, sin espacios ni acentos ni mayúsculas |
| Contenido | `<h2>`, `.bajada`, `.cuerpo` | Título, copete y cuerpo |

El nombre visible de la categoría (`Tecnología`, `Política`, `Govtech`) sale
solo de `data-cat`. El `<span class="cat">` del artículo ya no se muestra.

### Lo que se genera solo

No lo escribas, se calcula al cargar la página:

- La fecha en formato legible (`14 ago 2026`).
- El tiempo de lectura, estimado en 200 palabras por minuto.
- El iris que acompaña a cada nota en la lista, único para cada `id`.
- El orden de las notas, de la más nueva a la más vieja.
- Los contadores por categoría de la pantalla de inicio.

### El orden no importa

Pegá la nota nueva donde te resulte cómodo. La página ordena por `data-fecha`.
Si dos notas comparten fecha, se ordenan como estén escritas en el archivo.

### El `id` es el link permanente

Una nota con `id="expediente-digital"` es accesible en
`https://tuusuario.github.io/#expediente-digital`. Ese link se puede compartir y
abre directo en esa nota. Las listas también tienen su link: `#t/tecnologia`,
`#t/politica`, `#t/govtech` y `#t/todo`.

**No cambies el `id` de una nota ya publicada.** Si lo cambiás, todos los links
que compartiste dejan de funcionar.

### Qué lee la página de cada artículo

Solo `data-cat`, `data-fecha`, `id`, el `<h2>`, la `.bajada` y el `.cuerpo`. El
resto (`.post-meta`, el botón `Leer nota`) queda sin uso, pero no hace falta
borrarlo de las notas que ya están.

### Formato dentro del cuerpo

```html
<p>Un párrafo. Cada párrafo va entre p y /p.</p>

<h3>Un subtítulo dentro de la nota</h3>

<blockquote>Una cita destacada. Se muestra con una línea vertical
del color de la categoría.</blockquote>

<ul>
  <li>Un ítem de lista</li>
  <li>Otro ítem</li>
</ul>

<p>Texto con <a href="https://ejemplo.com">un enlace</a> y con
<em>énfasis</em> o <strong>negrita</strong>.</p>
```

### Imágenes

No son necesarias: cada nota ya tiene su iris generado. Usalas cuando la imagen
sea información — un gráfico, una captura, una tabla.

1. Creá una carpeta `imagenes/` en el repositorio y subí el archivo ahí.
2. Guardala en `.webp` y a 1400 píxeles de ancho como máximo.
3. Insertala dentro del `.cuerpo`:

```html
<figure>
  <img src="imagenes/presupuesto-2026.webp" alt="Descripción de la imagen">
  <figcaption>Fuente: Legislatura de Córdoba</figcaption>
</figure>
```

El `alt` no es decorativo: es lo que leen los lectores de pantalla y lo que
Google usa para entender la imagen. Describí qué se ve, no repitas el epígrafe.

---

## 3. Cómo funciona la página

### Todo vive en el archivo

No hay servidor ni base de datos. Las notas son texto dentro de `index.html`.
Nada expira, nada se borra, nada se cae. Tu respaldo es una copia del archivo.

Las notas viven en `<main id="lista" hidden>`: ese bloque queda oculto y la
página lo lee para armar las pantallas. Sin JavaScript no se ve ninguna nota.

### Las tres pantallas

El ojo es la navegación del blog. Hay tres pantallas, y la dirección (lo que
va después del `#`) dice cuál se ve:

- **Inicio** (`#`): el ojo grande al centro y debajo las opciones: Tecnología,
  Política, Govtech y Todo, cada una con su cantidad de notas. Pasar el mouse
  por una opción tiñe el ojo de su color y lo hace mirarla.
- **Lista** (`#t/tecnologia`, `#t/politica`, `#t/govtech`, `#t/todo`): el ojo
  se corre a la izquierda y las notas de ese tipo entran de una en una desde la
  derecha. Arriba hay pestañas para cambiar de tipo sin volver al inicio.
- **Nota** (`#id-de-la-nota`): la página baja y la nota se lee con el ojo fijo
  al costado, mirando el texto según cuánto se avanzó. En celular el ojo queda
  arriba.

Cada tipo muestra todas sus notas: ya no hay portada con corte ni archivo.
`Escape` vuelve un paso atrás y el botón atrás del navegador también.

### Buscador

Filtra mientras se escribe, sobre el título, la bajada y el cuerpo completo.
Ignora acentos y mayúsculas. Escribir desde el inicio o desde una nota lleva a
`Todo` con los resultados; dentro de una lista, filtra esa lista. Como todo el
contenido ya está en la página, la búsqueda es instantánea y no consulta
ningún servidor.

### El ojo

Está hecho con unos 1700 triángulos diminutos dibujados en un `<canvas>` que
ocupa toda la ventana, más unos 150 triángulos sueltos de fondo. El ojo se
dibuja donde esté el recuadro `#ojo` de la página: al cambiar de pantalla, el
recuadro cambia de lugar y de tamaño, y los triángulos fluyen hasta el nuevo
lugar, cada uno con su propio resorte.

- **Mirada:** sigue al cursor con inercia, parpadea de vez en cuando y, sin
  cursor (celular), recorre la escena sola.
- **Cursor:** los triángulos cercanos se apartan al pasar el mouse.
- **Color:** el de la categoría que se mira. En `Todo` y en el inicio, mezcla
  los cuatro.
- **Equipo lento:** si los cuadros tardan demasiado, el ojo reduce solo la
  cantidad de triángulos.
- **Movimiento reducido:** si el sistema del lector lo pide, el ojo se dibuja
  quieto.

Para ajustarlo, en el CSS (tamaño en cada pantalla):

```css
body[data-vista="inicio"] .ojo-slot { width: min(640px, 52vw, 78vh); }   /* inicio */
body:not([data-vista="inicio"]) .ojo-slot { max-width: 540px; }          /* lista */
body[data-vista="nota"] .ojo-slot { max-width: 380px; }                  /* nota */
```

Y en el último `<script>` (el del ojo): `K` (rigidez del resorte), `R_MOUSE` y
`F_MOUSE` (alcance y fuerza con que el cursor aparta los triángulos) y `ADITIVO`
(si es `true`, donde se juntan brillan más).

### Colores

Están todos declarados juntos al principio del CSS, en `:root`. Cambiar uno
cambia todos los lugares donde se usa.

```css
--ink:        #000000;   /* fondo */
--paper:      #ffffff;   /* texto y títulos */
--dim:        #9a9a9a;   /* texto secundario */
--mist:       #bdbdbd;   /* bajadas */
--lectura:    #ededed;   /* cuerpo de las notas */
--iris:       #8052ff;   /* violeta de la marca */
--tecnologia: #FFC738;   /* amarillo */
--politica:   #FF634B;   /* naranja rojizo */
--govtech:    #6CAA6D;   /* verde */
```

Los colores de las tres categorías y el violeta también están copiados, en
RGB, en la lista `BASE` del script del ojo. Si los cambiás en `:root`, cambialos
ahí también.

### Tipografías

**Inter** para títulos, menú y etiquetas, y **Newsreader** (con serif) para el
cuerpo de las notas, donde se lee mejor un texto largo. Se cargan desde Google
Fonts: es la única cosa que la página descarga desde afuera.

El estilo visual parte del sistema "Dala" de [Refero Styles](https://styles.refero.design):
negro puro, títulos grandes en peso 400, sin bordes ni tarjetas, y un solo
violeta para la acción principal. Se mantuvieron los colores por categoría.

### Peso

Con 10 notas el archivo pesa unos 120 KB. Cada nota suma entre 4 y 15 KB (unos
8 KB de promedio). Con cincuenta notas rondás los 450 KB: sigue siendo menos que
una sola foto de celular.

---

## 4. Recomendaciones

### Antes de publicar (ya hecho)

- **Artículos de ejemplo:** ya se borraron todos.
- **Contacto del pie:** ya no hay correo de ejemplo; el pie enlaza a
  [@orwelliano90](https://x.com/orwelliano90) en X.
- **Descripción del sitio:** la etiqueta `<meta name="description">` del
  encabezado ya está escrita, junto con las etiquetas Open Graph y Twitter Card
  que definen la vista previa al compartir (`imagenes/Eye_Illustration.jpg`).
  Si cambia el enfoque del blog, actualizá esos textos.

### Sobre la escritura

- **La bajada hace casi todo el trabajo.** En la lista de cada tipo, el lector ve título y
  bajada. Si la bajada resume la nota en lugar de dar una razón para entrar, no
  entra. Que sea una afirmación, no un anuncio.
- **Publicá con ritmo irregular sin culpa.** La página está diseñada para eso:
  el inicio muestra el volumen real por categoría, sin esconder ninguna.
- **Una nota corta publicada le gana a una larga sin terminar.** El formato
  aguanta perfectamente notas de 400 palabras.

### Sobre la escala

- **Hasta 50 notas:** así como está, sin cambios. Cada lista muestra todas las
  notas de su tipo, sin corte, así que con muchas se hace larga.
- **A partir de ahí:** el problema no va a ser la velocidad sino tu comodidad
  editando un archivo de miles de líneas. Ese es el momento de migrar a un
  generador estático (Astro o Eleventy), con una nota por archivo en Markdown.
  Los `id` se pueden conservar para no romper los links.

### Lo que sumaría después

En orden de utilidad, si el blog agarra ritmo:

1. **Feed RSS.** Un archivo `feed.xml` que permite seguir el blog desde un
   lector. Barato de hacer y es lo que espera el público que lee cosas así.
2. **Vista previa al compartir.** Una imagen `og:image` para que el link no
   aparezca pelado en WhatsApp, LinkedIn y X.
3. **Suscripción por correo.** Un servicio externo (Buttondown, por ejemplo)
   para que quien quiera reciba las notas nuevas. Vale la pena solo cuando ya
   hay lectores que vuelven.
4. **Analítica liviana.** Si te interesa saber qué se lee. Evitá Google
   Analytics: pesa y rastrea. Alternativas como GoatCounter no hacen ninguna de
   las dos cosas.

### Tres advertencias

- **Las notas viven en un bloque oculto.** Google puede darle menos peso a
  texto oculto, y el blog no muestra nada sin JavaScript. Conviene mirar cada
  tanto en Google Search Console si las notas siguen apareciendo en las
  búsquedas. Si dejan de aparecer, es el momento de pasar a un generador
  estático (ver "Sobre la escala").

- **Nunca pongas una clave de API dentro del HTML.** El archivo es público y
  cualquiera puede leerlo. Si en algún momento querés generar contenido con IA
  desde la página, hace falta un intermediario que guarde la clave del lado del
  servidor.
- **Guardá una copia del archivo fuera de GitHub.** El historial de commits te
  cubre casi todo, pero una copia en tu disco o en Drive no cuesta nada.
