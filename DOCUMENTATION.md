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

Bootstrap, Font Awesome y Stylelint todavía no están instalados.

### Entorno de desarrollo

`.nvmrc` fija Node `v20.18.1`. Se ha comprobado que `node -v` devuelve `v24.20.0`, así que el desarrollo se hace con la versión indicada en `.nvmrc`.

El servidor de desarrollo es `npm run dev` (Parcel en el puerto 8123). Esta ejecución todavía no se ha registrado.

### Compilación para producción

El comando previsto es `npm run build`. Esta compilación todavía no se ha ejecutado. La salida publicable queda en `dist/`.

### Publicación

Todavía no se ha creado un repositorio propio ni un sitio en Netlify. Los enlaces se añaden en el apartado final cuando existan.

## Metodología y guía de estilo

### Justificación

Todavía no hay resultados que registrar en este apartado.

### Aplicación en el código

Todavía no hay resultados que registrar en este apartado.

### Stylelint

Todavía no hay resultados que registrar en este apartado.

## Dependencias

### Bootstrap

Todavía no hay resultados que registrar en este apartado.

### Otra dependencia

Todavía no hay resultados que registrar en este apartado.

## Decisiones de diseño y desarrollo

Todavía no hay resultados que registrar en este apartado.

## Enlaces

- Repositorio de GitHub: pendiente.
- Sitio público en Netlify: pendiente.
