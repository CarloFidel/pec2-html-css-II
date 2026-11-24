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

### Entorno de desarrollo

`.nvmrc` fija Node `v20.18.1`. Se ha comprobado que `node -v` devuelve `v24.20.0`, así que el desarrollo se hace con la versión indicada en `.nvmrc`.

Se ha ejecutado `npm run dev`. Parcel sirve el proyecto en el puerto 8123. En esa página el botón `.btn-primary` y la barra de navegación usan el color `rgb(107, 45, 60)`, que corresponde a `#6b2d3c`. El icono de Font Awesome se pinta con la familia `Font Awesome 7 Free`.

### Compilación para producción

Se ha ejecutado `npm run build`. La primera compilación ha avisado de que silenciar la deprecación `mixed-decls` ya no procede. Se ha quitado esa clave de `.sassrc.js` y se han mantenido `import`, `color-functions` y `global-builtin`. La compilación siguiente ha terminado sin avisos de Sass y ha generado `dist/`, con el HTML, el CSS, el JavaScript y las fuentes de Font Awesome.

### Publicación

Todavía no se ha creado un repositorio propio ni un sitio en Netlify. Los enlaces se añaden en el apartado final cuando existan.

## Metodología y guía de estilo

### Justificación

Se ha elegido BEM como metodología de nomenclatura. El encargo es un sitio de cuatro páginas (portada, ponentes, artículo y una página de contenido libre) sobre unas jornadas y un concurso literario, con cabecera y pie compartidos y con bloques que se repiten: la ficha de ponente, el póster, la figura del artículo y el formulario. BEM da a cada uno de esos bloques un nombre único (`speaker`, `poster`, `article`) y separa el elemento (`speaker__name`) del modificador (`speaker--featured`). El mismo patrón sirve en las cuatro páginas sin depender del contexto en el que está colocado el bloque.

El boilerplate ya fija las herramientas y la estructura de carpetas (`src/assets/styles/`, un parcial por página en `layouts/`). BEM no pide reordenar esos ficheros: la decisión que quedaba abierta era el nombre de las clases. Esa decisión encaja con el código que se va a escribir, de especificidad baja y basado en clases. Las clases de Bootstrap (`.navbar`, `.btn`, `.card`, `.form-control`) se mantienen como las entrega la librería. Los bloques propios del proyecto llevan nombres BEM, también la retícula de ponentes, que se maqueta con flex y con clases del proyecto.

De las metodologías del módulo, OOCSS separa estructura y piel en clases reutilizables sueltas; aquí cada ficha y cada sección de página se tratan como un bloque con su marcado. SMACSS e ITCSS clasifican u ordenan las reglas en capas de ficheros. Con cuatro páginas y la estructura del boilerplate ya establecida, esa reorganización no aporta tanto como un criterio de nombres comprobable. BEM cubre ese criterio y se puede validar después con `selector-class-pattern` en Stylelint.

### Aplicación en el código

Todavía no hay resultados que registrar en este apartado.

### Stylelint

Se ha creado `.stylelintrc` en la raíz. El módulo 2 pide ese nombre de fichero. La configuración extiende `stylelint-config-standard-scss`, que ya incorpora `stylelint-scss`.

La regla que aplica BEM es `selector-class-pattern`, con la expresión del módulo: `^.[a-z]([a-z0-9-]+)?(__([a-z0-9]+-?)+)?(--([a-z0-9]+-?)+){0,2}$`. No se ha dejado en `null`: el enunciado pide que Stylelint vigile el criterio elegido, y las clases de Bootstrap viven en el HTML y en `node_modules`, fuera de esta orden. Una clase como `.site-header__nav` cumple el patrón; una clase de una sola palabra también, porque el elemento y el modificador son opcionales.

Para las técnicas del módulo 3 se mantienen estas reglas:

- `selector-pseudo-class-no-unknown` admite `:has`, `:is`, `:where` y `:global`.
- `at-rule-no-unknown` queda desactivada y `scss/at-rule-no-unknown` admite `@layer`, `@container` y `@supports`.
- `no-descending-specificity` queda desactivada.
- `color-function-notation` es `modern` y `alpha-value-notation` es `percentage`.

No se han desactivado `scss/dollar-variable-pattern`, `custom-property-pattern` ni `scss/at-mixin-pattern`. Las variables del proyecto ya usan guiones (`$literary-primary`) y esas reglas no chocan con Bootstrap.

Los scripts son `stylelint`, `lint:css` y `lint`. Los tres ejecutan `stylelint "src/**/*.scss"`, la carpeta real de los estilos. `npm run build` lanza Stylelint antes de Parcel. `npm run stylelint` ha terminado sin mensajes. Para llegar a ese resultado se han corregido espacios tras los dos puntos en `_variables.scss`, el color `#ffffff`, la sombra de `_home.scss` y el prefijo `-webkit-text-size-adjust` de `_reset.scss`.

## Dependencias

### Bootstrap

Bootstrap 5.3.9 se carga desde Sass, no desde el CSS ya compilado. `_dependencies.scss` queda reservado a librerías de terceros. La personalización vive en `src/assets/styles/_bootstrap.scss`: primero `@import "variables"` y después `@import "bootstrap/scss/bootstrap"`. `main.scss` carga ese parcial con `@use "bootstrap"`. En `src/assets/scripts/main.js` se importa el JavaScript con `import * as bootstrap from "bootstrap"`. `@popperjs/core` 2.11.8 queda como dependencia de los componentes que lo necesitan, como el menú colapsable.

`_variables.scss` define la paleta que usa la portada (`$literary-primary` `#6e2e3c`, superficies, tintas y el par `$badge-talleres-bg` / `$badge-talleres-color`) y, antes de importar Bootstrap, reasigna `$primary`, `$secondary`, `$body-bg`, `$body-color`, `$font-family-sans-serif`, `$font-family-serif`, `$font-family-base`, `$font-size-base`, `$line-height-base`, `$border-color`, `$border-radius`, `$border-radius-lg`, `$border-radius-xl`, `$box-shadow`, `$box-shadow-sm`, `$btn-border-radius`, `$btn-font-weight`, `$btn-padding-y`, `$btn-padding-x`, `$card-bg`, `$card-border-color`, `$card-border-radius` y `$theme-colors`. En ese mapa el primario es `$literary-primary` y el secundario es el ámbar. Se conservan `success`, `info`, `warning` y `danger`. El orden de capas se declara con `@layer reset, framework, components, utilities`. Bootstrap no entra en esas capas. Inter y Newsreader se cargan con `@font-face` en `src/assets/styles/_fonts.scss`, importado desde `main.scss`. `$font-family-base` aplica Inter al texto y `$headings-font-family` aplica Newsreader a los títulos de Bootstrap.

En `src/index.html` se comprueban cinco componentes: navbar (con `navbar-toggler` y `data-bs-toggle="collapse"`), botón (`.btn` y `.btn-primary`), alerta (`.alert`), tarjeta (`.card`) y formulario (`.form-label` y `.form-control`). Esta página es temporal: el reparto definitivo entre portada, ponentes, artículo y contacto se hace cuando existan esas páginas. La retícula de ponentes no usa `.row` ni `.col-*`.

### Otra dependencia

Font Awesome 7.3.1 se importa en `src/assets/styles/_dependencies.scss` con `@import "npm:@fortawesome/fontawesome-free/css/all.css"`. `main.scss` carga ese parcial con `@use "dependencies"`. En la portada temporal se usan los iconos `fa-book-open`, en la marca de la barra, y `fa-pen-nib`, en el botón de inscripción.

## Decisiones de diseño y desarrollo

Todavía no hay resultados que registrar en este apartado.

## Enlaces

- Repositorio de GitHub: pendiente.
- Sitio público en Netlify: pendiente.
