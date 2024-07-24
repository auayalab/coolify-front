# frontend

## Requisitos Previos

- [Node.js](https://nodejs.org/) (versión 18.x o superior)
- [yarn](https://yarnpkg.com/)
- [Git](https://git-scm.com/)

## Instalación 

- Cambia la url base en nuxt.config.js en: 

axios: {
    baseURL: "http://direccion-del-proyecto/api",
  }, 

- y en: 

publicRuntimeConfig: {
    baseURL: "http:///direccion-del-proyecto/",
  },

## Build Setup

```bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn dev

# build for production and launch server
$ yarn build
$ yarn start

# generate static project
$ yarn generate
```

For detailed explanation on how things work, check out [Nuxt.js docs](https://nuxtjs.org).
