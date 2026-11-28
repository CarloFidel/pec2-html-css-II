# Documentación de la PEC 2

Sitio web de las jornadas y el concurso literario. Esta memoria recoge el proceso de desarrollo, los criterios de estilo, las dependencias, las decisiones de diseño y los enlaces públicos.

## Proceso de desarrollo

### Punto de partida

El desarrollo parte de UOC Boilerplate 3.14.0. En `package.json` el nombre del paquete es `uoc-boilerplate` y la versión es `3.14.0`. La estructura de carpetas del boilerplate se mantiene: el HTML entra por `src/index.html`, los estilos por `src/assets/styles/main.scss`, los parciales de página en `src/assets/styles/layouts/`, los scripts en `src/assets/scripts/main.js` y los fragmentos HTML en `src/views/`.

### Instalación de dependencias

Se han instalado las dependencias de desarrollo del boilerplate. `npm ls --depth=0` devuelve:

- `@parcel/transformer-sass` 2.16.4
- `autoprefixer` 10.5.5
- `npm-run-all` 4.1.5
- `parcel` 2.16.4
- `postcss-preset-env` 11.5.1
- `posthtml-include` 2.0.1
- `rimraf` 6.1.3
- `sharp` 0.33.5

Además se han instalado, con `npm install @popperjs/core bootstrap @fortawesome/fontawesome-free`:

- `bootstrap` 5.3.9
- `@popperjs/core` 2.11.8
- `@fortawesome/fontawesome-free` 7.3.1

También se han instalado, como dependencias de desarrollo:

- `stylelint` 18.1.0
- `stylelint-scss` 7.5.0
- `stylelint-config-standard-scss` 18.0.0
- `stylelint-config-recess-order` 7.10.2

### Entorno de desarrollo

`.nvmrc` fija Node `v20.18.1`. Se ha comprobado que `node -v` devuelve `v24.20.0`; el entorno local usado para compilar y servir el proyecto ha sido esa versión, aunque el fichero del boilerplate siga recomendando la LTS anterior.

Se ha ejecutado `npm run dev`. Parcel sirve el proyecto en el puerto 8123. En esa página el botón `.btn-primary` y la barra de navegación aplican el primario del tema, `$literary-primary` (`#6e2e3c`, asignado a `$primary` en Sass). El icono de Font Awesome se pinta con la familia `Font Awesome 7 Free`.

### Compilación para producción

Se ha ejecutado `npm run build`. La primera compilación ha avisado de que silenciar la deprecación `mixed-decls` ya no procede. Se ha quitado esa clave de `.sassrc.js` y se han mantenido `import`, `color-functions` y `global-builtin`. Antes de Parcel, el script lanza Stylelint sobre `src/**/*.scss`.

La última compilación ha terminado sin avisos de Sass y ha generado `dist/` con cuatro HTML (`index.html`, `ponentes.html`, `jornadas.html` y `contacto.html`), un CSS bundled, JavaScript, fuentes locales y las fuentes de Font Awesome.

### Publicación

- Repositorio: https://github.com/CarloFidel/pec2-html-css-II
- Sitio público: https://effortless-strudel-7fb016.netlify.app/

El remoto de Git apunta a ese repositorio en GitHub. El sitio se publica en Netlify con comando de build `npm run build` y directorio de publicación `dist`. Las cuatro páginas quedan disponibles en la raíz del dominio (`index.html`, `ponentes.html`, `jornadas.html`, `contacto.html`). La prueba de envío del formulario con atributo `netlify` en el panel de Forms de Netlify queda como comprobación manual antes de entregar.

## Metodología y guía de estilo

### Justificación

Se ha elegido BEM como metodología de nomenclatura. El encargo es un sitio de cuatro páginas (portada, ponentes, artículo y una página de contenido libre) sobre unas jornadas y un concurso literario, con cabecera y pie compartidos y con bloques que se repiten: la ficha de ponente, el póster, la figura del artículo y el formulario. BEM da a cada uno de esos bloques un nombre único (`speaker`, `poster`, `article`) y separa el elemento (`speaker__name`) del modificador (`speaker--featured`). El mismo patrón sirve en las cuatro páginas sin depender del contexto en el que está colocado el bloque.

El boilerplate ya fija las herramientas y la estructura de carpetas (`src/assets/styles/`, un parcial por página en `layouts/`). BEM no pide reordenar esos ficheros: la decisión que quedaba abierta era el nombre de las clases. Esa decisión encaja con el código que se va a escribir, de especificidad baja y basado en clases. Las clases de Bootstrap (`.navbar`, `.btn`, `.card`, `.form-control`) se mantienen como las entrega la librería. Los bloques propios del proyecto llevan nombres BEM, también la retícula de ponentes, que se maqueta con flex y con clases del proyecto.

De las metodologías del módulo, OOCSS separa estructura y piel en clases reutilizables sueltas; aquí cada ficha y cada sección de página se tratan como un bloque con su marcado. SMACSS e ITCSS clasifican u ordenan las reglas en capas de ficheros. Con cuatro páginas y la estructura del boilerplate ya establecida, esa reorganización no aporta tanto como un criterio de nombres comprobable. BEM cubre ese criterio y se puede validar después con `selector-class-pattern` en Stylelint.

### Aplicación en el código

En `src/views/header.html` y `src/views/footer.html` los bloques compartidos son `site-header` (`site-header__brand`, `site-header__mark`, `site-header__title`, `site-header__kicker`) y `site-footer` (`site-footer__bar`, `site-footer__tab`, `site-footer__panel`, `site-footer__link` y el resto de elementos con prefijo `site-footer__`). La navegación combina esas clases con las de Bootstrap (`.navbar`, `.nav-link`, `.collapse`).

En `src/index.html` la portada agrupa el bloque `home-hero`: el póster en `home-hero__poster`, el antetítulo en `home-hero__title__subtitle`, la cita en `home-hero__quote`, el bloque de tarjetas en `home-hero__cards` y cada tarjeta en `home-hero__card`. Los modificadores de icono y cifra son `home-hero__icon--blue`, `home-hero__icon--amber`, `home-hero__figure--primary`, `home-hero__figure--amber` y `home-hero__figure--title`. Las cuatro tarjetas de resumen usan `.row` y `.col-6` de Bootstrap.

En `src/ponentes.html` hay seis fichas. La lista es el bloque `speakers` (`speakers__list`) y cada ficha es el bloque `speaker`. Los elementos con nombre propio son `speaker__body`, `speaker__photo`, `speaker__kicker` y `speaker__name`. El horario y el lugar son dos líneas con icono de Font Awesome y la clase `.text-muted`. No hay modificador. La lista no usa `.row` ni `.col-*`. Bajo la lista, `.alert` dice que se pueden añadir más ponentes.

En `src/jornadas.html` el bloque es `article`. Los elementos son `article__author`, `article__lead`, `article__steps-wrap`, `article__steps`, `article__step-badge`, `article__highlight`, `article__quote`, `article__figure`, `article__figure-photo` y `article__caption`. Los estilos de página viven en `src/assets/styles/layouts/_jornadas.scss`. En `src/contacto.html` el bloque es `contact`, con `contact__intro`, `contact__form`, `contact__fields` y `contact__field--full`. El formulario usa las clases de Bootstrap `.form-label`, `.form-control` y `.btn`.

### Stylelint

Se ha creado `.stylelintrc` en la raíz. El módulo 2 pide ese nombre de fichero. La configuración extiende `stylelint-config-standard-scss`, que ya incorpora `stylelint-scss`, y `stylelint-config-recess-order`, que ordena las propiedades según la Code Guide de @mdo.

La regla que aplica BEM es `selector-class-pattern`, con la expresión del módulo: `^.[a-z]([a-z0-9-]+)?(__([a-z0-9]+-?)+)?(--([a-z0-9]+-?)+){0,2}$`. No se ha dejado en `null`: el enunciado pide que Stylelint vigile el criterio elegido, y las clases de Bootstrap viven en el HTML y en `node_modules`, fuera de esta orden. Una clase como `.site-header__nav` cumple el patrón; una clase de una sola palabra también, porque el elemento y el modificador son opcionales.

Para las técnicas del módulo 3 se mantienen estas reglas:

- `selector-pseudo-class-no-unknown` admite `:has`, `:is`, `:where` y `:global`.
- `at-rule-no-unknown` queda desactivada y `scss/at-rule-no-unknown` admite `@layer`, `@container` y `@supports`.
- `no-descending-specificity` queda desactivada.
- `color-function-notation` es `modern` y `alpha-value-notation` es `percentage`.

No se han desactivado `scss/dollar-variable-pattern`, `custom-property-pattern` ni `scss/at-mixin-pattern`. Las variables del proyecto ya usan guiones (`$literary-primary`) y esas reglas no chocan con Bootstrap.

Los scripts son `stylelint`, `lint:css` y `lint`. Los tres ejecutan `stylelint "src/**/*.scss"`, la carpeta real de los estilos. `npm run build` lanza Stylelint antes de Parcel. Se ha ejecutado `npm run stylelint` tras añadir `stylelint-config-recess-order` y ha terminado sin mensajes.

## Dependencias

### Bootstrap

Bootstrap 5.3.9 se carga desde Sass, no desde el CSS ya compilado. `_dependencies.scss` queda reservado a librerías de terceros. La personalización vive en `src/assets/styles/_bootstrap.scss`: primero `@import "variables"` y después `@import "bootstrap/scss/bootstrap"`. `main.scss` carga ese parcial con `@use "bootstrap"`. En `src/assets/scripts/main.js` se importa el JavaScript con `import * as bootstrap from "bootstrap"`. `@popperjs/core` 2.11.8 queda como dependencia de los componentes que lo necesitan, como el menú colapsable.

`_variables.scss` define la paleta que usa la portada (`$literary-primary` `#6e2e3c`, superficies, tintas y el par `$badge-talleres-bg` / `$badge-talleres-color`), la función `fluid($min, $max)` con `clamp()` para tamaños fluidos (por ejemplo el antetítulo del póster en `layouts/_home.scss`) y, antes de importar Bootstrap, reasigna `$primary`, `$secondary`, `$body-bg`, `$body-color`, `$font-family-sans-serif`, `$font-family-serif`, `$font-family-base`, `$font-size-base`, `$line-height-base`, `$border-color`, `$border-radius`, `$border-radius-lg`, `$border-radius-xl`, `$box-shadow`, `$box-shadow-sm`, `$btn-border-radius`, `$btn-font-weight`, `$btn-padding-y`, `$btn-padding-x`, `$card-bg`, `$card-border-color`, `$card-border-radius` y `$theme-colors`. En ese mapa el primario es `$literary-primary` y el secundario es el ámbar. Se conservan `success`, `info`, `warning` y `danger`. El orden de capas se declara con `@layer reset, framework, components, utilities`. Bootstrap no entra en esas capas. Inter y Newsreader se cargan con `@font-face` en `src/assets/styles/_fonts.scss`, importado desde `main.scss`. `$font-family-base` aplica Inter al texto y `$headings-font-family` aplica Newsreader a los títulos de Bootstrap.

En `src/index.html` se comprueban navbar (con `navbar-toggler` y `data-bs-toggle="collapse"`), botón (`.btn` y `.btn-primary`) y tarjeta (`.card`). En ponentes, la ficha es `.card` y el aviso es `.alert`. El formulario (`.form-label`, `.form-control` y `.btn`) está en `src/contacto.html`. La retícula de ponentes no usa `.row` ni `.col-*`.

### Otra dependencia

Font Awesome 7.3.1 se importa en `src/assets/styles/_dependencies.scss` con `@import "npm:@fortawesome/fontawesome-free/css/all.css"`. `main.scss` carga ese parcial con `@use "dependencies"`. En `src/views/header.html` la marca usa `fa-book-open`. En la portada, cada tarjeta lleva `fa-calendar`, `fa-pen`, `fa-trophy` o `fa-list`. En ponentes, el horario y el lugar muestran `fa-clock` y `fa-building-columns`. El pie móvil y el bloque inferior en escritorio reutilizan iconos de navegación y contacto (`fa-users`, `fa-file-lines`, `fa-feather`, `fa-envelope` y otros del mismo bloque).

## Decisiones de diseño y desarrollo

La portada maqueta el póster en `.home-hero__poster`. Sin CSS Grid el bloque es una columna flex, en el orden del HTML: antetítulo, título, cita y foto a todo el ancho. Dentro de `@supports (display: grid)` las mismas piezas ocupan una sola columna, pero las celdas colocan la foto primero (`image`, `kicker`, `title`, `quote`). El título es un enlace a `ponentes.html` y sigue heredando el crema del fondo con `text-reset`. `.home-hero__poster` es un contenedor (`container-type: inline-size`). El `h1` mide `1.75rem` y, desde `32rem` de ancho del póster, `2.75rem`.

Hay seis fichas, cada una una `.card`. El orden interior es foto, cátedra, nombre, dos líneas de texto, horario y lugar. Esas dos últimas son texto suelto, sin fondo ni borde. `speakers__list` es flex con `flex-wrap` y `gap: 1rem`. Cada ítem ocupa el 100 %; desde `48rem` caben dos por fila y desde `72rem`, tres. El `flex-basis` resta el hueco. Cada `.speaker` es un contenedor (`container-type: inline-size`). Por debajo de `28rem` la foto va encima del texto; desde ese ancho, `@container` pone `.speaker__body` en fila y la foto pasa a `7rem`. Esas reglas están en `@layer components`. La proporción de la foto en columna es `aspect-ratio: 4 / 3`. Las fotos salen de `speaker_01.jpeg` a `speaker_05.jpeg`; `speaker.jpeg` se reutiliza en dos fichas. El color y el espaciado de la ficha salen de utilidades de Bootstrap. El aviso bajo la retícula es `.alert` y `.alert-light`.

La página del artículo abre con la foto `jornadas.jpeg`, el título «Jornadas» y el nombre de la autora en `article__author`. El titular y el primer párrafo van en fila desde el breakpoint `md` de Bootstrap y en columna por debajo. Hay cuatro ítems en la lista con marcador `article__step-badge`, una cita, varios párrafos y una segunda foto, `book_01.jpeg`, dentro de `<figure>` con `<figcaption>`. La figura del cuerpo lleva pie y recibe borde inferior con `figure:has(figcaption)`. `:is(h2, h3)` aplica la serif del proyecto. `.article__figure` es contenedor. Por debajo de `40rem` el pie queda bajo la foto; desde ese ancho la figura pasa a fila. Las imágenes del artículo llevan `max-width: 100%`. Esas reglas están en `@layer components`.

La página de contacto es la sección `.contact`, con `min-height: 100svh`, fondo `oklch(96% 0.02 80deg)` y texto `oklch(28% 0.04 30deg)`. El formulario tiene los campos nombre, correo y mensaje, los atributos `data-netlify` y `netlify`, `method="post"`, `action="./contacto.html"` y un `name` en cada control (`nombre`, `correo`, `mensaje`). Los campos nombre y correo ocupan dos columnas desde `32rem` de ancho del formulario gracias al container query en `.contact__form`; el mensaje usa `contact__field--full` a ancho completo. Los controles tienen altura mínima para facilitar el toque en móvil.

## Enlaces

- Repositorio de GitHub: https://github.com/CarloFidel/pec2-html-css-II
- Sitio público en Netlify: https://effortless-strudel-7fb016.netlify.app/
