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

El `<span class="cat">` de la primera línea es el nombre visible de la
categoría. Ponelo con acento y mayúscula: `Tecnología`, `Política`, `Govtech`.

### Lo que se genera solo

No lo escribas, se calcula al cargar la página:

- La fecha en formato legible (`14 ago 2026`).
- El tiempo de lectura, estimado en 200 palabras por minuto.
- El iris de la derecha, único para cada `id`.
- El orden de las notas, de la más nueva a la más vieja.
- Los contadores por categoría de la barra superior.
- El corte del archivo.

### El orden no importa

Pegá la nota nueva donde te resulte cómodo. La página ordena por `data-fecha`.
Si dos notas comparten fecha, se ordenan como estén escritas en el archivo.

### El `id` es el link permanente

Una nota con `id="expediente-digital"` es accesible en
`https://tuusuario.github.io/#expediente-digital`. Ese link se puede compartir y
abre directo en esa nota.

**No cambies el `id` de una nota ya publicada.** Si lo cambiás, todos los links
que compartiste dejan de funcionar.

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

### Portada y archivo

Las **10 notas más nuevas** se muestran completas. A partir de la 11, pasan
automáticamente a un índice compacto de una línea, bajo el título `ARCHIVO`. Es
la misma nota, mostrada de otra forma: se abre igual con un clic.

Para cambiar ese corte, buscá en el JavaScript:

```js
var LIMITE_PORTADA = 10;
```

### Buscador

Filtra mientras se escribe, sobre el título, la bajada y el cuerpo completo.
Ignora acentos y mayúsculas. Como todo el contenido ya está en la página, la
búsqueda es instantánea y no consulta ningún servidor.

Cuando hay una búsqueda o un filtro activo, el archivo se desarma y todas las
coincidencias se muestran como fichas completas.

### Los dos ojos

- **El chico, rojo, arriba:** la marca del blog. Su pupila sigue al cursor.
- **El grande, detrás del nombre:** marca de agua al 7% de opacidad, con la
  pupila siguiendo al cursor a la mitad de recorrido, lo que da profundidad.

Ambos se desactivan solos en pantallas táctiles y si el sistema del lector tiene
activada la reducción de movimiento.

Para ajustarlos, en el CSS:

```css
.ojo { width: clamp(90px, 15vw, 168px); }   /* el ojo chico */
.ojo-fondo { opacity: .07; }                /* la marca de agua */
```

### Colores

Están todos declarados juntos al principio del CSS, en `:root`. Cambiar uno
cambia todos los lugares donde se usa.

```css
--ink:        #060606;   /* fondo */
--paper:      #E3E3E3;   /* texto */
--dim:        #8A8A8A;   /* texto secundario */
--tecnologia: #FFC738;   /* amarillo */
--politica:   #FF634B;   /* naranja rojizo */
--govtech:    #6CAA6D;   /* verde */
```

### Tipografías

**Archivo** para títulos (Omnibus-Type, Buenos Aires) y **Newsreader** para el
cuerpo. Se cargan desde Google Fonts: es la única cosa que la página descarga
desde afuera. Son los sustitutos libres de GT America y Martina Plantijn, las
fuentes de pago que usa la página que sirvió de referencia.

### Peso

El archivo pesa unos 25 KB. Cada nota suma entre 3 y 5 KB. Con cincuenta notas
seguís por debajo de los 250 KB: menos que una sola foto de celular.

---

## 4. Recomendaciones

### Ahora, antes de publicar

- **Borrá los tres artículos de ejemplo.** Están marcados con
  `▼▼▼ EJEMPLO — borralo ▼▼▼`.
- **Cambiá el correo del pie.** Dice `tucorreo@ejemplo.com`.
- **Revisá la descripción del sitio.** La etiqueta `<meta name="description">`
  del encabezado es el texto que aparece en Google y en las vistas previas de
  WhatsApp.

### Sobre la escritura

- **La bajada hace casi todo el trabajo.** En la portada, el lector ve título y
  bajada. Si la bajada resume la nota en lugar de dar una razón para entrar, no
  entra. Que sea una afirmación, no un anuncio.
- **Publicá con ritmo irregular sin culpa.** La página está diseñada para eso:
  la barra de estado muestra el volumen real por categoría y nunca queda una
  sección vacía a la vista.
- **Una nota corta publicada le gana a una larga sin terminar.** El formato
  aguanta perfectamente notas de 400 palabras.

### Sobre la escala

- **Hasta 50 notas:** así como está, sin cambios.
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

### Dos advertencias

- **Nunca pongas una clave de API dentro del HTML.** El archivo es público y
  cualquiera puede leerlo. Si en algún momento querés generar contenido con IA
  desde la página, hace falta un intermediario que guarde la clave del lado del
  servidor.
- **Guardá una copia del archivo fuera de GitHub.** El historial de commits te
  cubre casi todo, pero una copia en tu disco o en Drive no cuesta nada.
