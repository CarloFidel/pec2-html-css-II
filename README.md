# Palabras al Viento — PEC 2 (HTML/CSS II)

Sitio estático de cuatro páginas para las jornadas literarias y el concurso **Palabras al Viento**: portada, ponentes, artículo (bases del certamen) y contacto con formulario.

- **Sitio publicado:** [effortless-strudel-7fb016.netlify.app](https://effortless-strudel-7fb016.netlify.app/)
- **Repositorio:** [github.com/CarloFidel/pec2-html-css-II](https://github.com/CarloFidel/pec2-html-css-II)
- **Memoria de la PEC:** [DOCUMENTATION.md](./DOCUMENTATION.md)

## Stack

- [Parcel](https://parceljs.org/) 2 (bundler y servidor de desarrollo)
- Sass/SCSS, [Bootstrap](https://getbootstrap.com/) 5.3 (personalizado por variables)
- [Font Awesome](https://fontawesome.com/) 7 (iconos)
- [Stylelint](https://stylelint.io/) (`stylelint-config-standard-scss`, `stylelint-config-recess-order`)

Parte del [UOC Boilerplate](https://github.com/uoc-advanced-html-css/uoc-boilerplate) 3.14.0.

## Requisitos

- Node.js ≥ 18 (en `.nvmrc` figura `v20.18.1`; comprobar con `node -v`)

## Instalación

```bash
npm install
```

## Comandos

| Comando | Descripción |
| --- | --- |
| `npm run dev` | Limpia caché y sirve en el puerto **8123** (recarga al cambiar archivos en `src/`) |
| `npm run build` | Ejecuta Stylelint y genera la carpeta `dist/` lista para producción |
| `npm run stylelint` | Revisa los SCSS en `src/**/*.scss` |
| `npm run clean` | Borra `dist/` y cachés de Parcel |

El despliegue en Netlify usa `npm run build` y publica el directorio `dist`.

## Páginas

| Ruta | Archivo fuente |
| --- | --- |
| Inicio | `src/index.html` |
| Ponentes y jurados | `src/ponentes.html` |
| Bases del certamen | `src/jornadas.html` |
| Concurso e inscripción | `src/contacto.html` |

Cabecera y pie compartidos: `src/views/header.html`, `src/views/footer.html` (incluidos con `posthtml-include`).

## Estructura relevante

```text
src/
├── assets/
│   ├── images/
│   ├── scripts/main.js    # Bootstrap JS
│   └── styles/
│       ├── main.scss
│       ├── layouts/         # Estilos por página
│       └── ...
├── views/                 # Fragmentos HTML
└── *.html
```

Metodología de clases propias: **BEM** (validada con `.stylelintrc`). Maquetado y utilidades: clases de Bootstrap en el HTML.

## Licencia

MIT (heredada del boilerplate UOC).