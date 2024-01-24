# Migrate to Vite

## Introduction

Vite has become the de facto standard for building frontend applications. With V6 release, we ship an official integration for using Vite inside AdonisJS applications.

We no longer recommend using Webpack Encore for new projects. However, we will continue to maintain this package for existing v5 applications.

If you'd like to migrate to Vite later, then you can install the `@adonisjs/encore` package which we maintain to enable you to migrate more easily to V6. However note that no further improvements will be made to Webpack integration. We therefore recommend that you migrate to Vite as soon as possible.

## Installation 

First you will need to install and configure `@adonisjs/vite` : 

```sh
pnpm add @adonisjs/vite
node ace configure @adonisjs/vite
```

This command will : 
- Create a `vite.config.ts` file at the root of your project
- Create a basic `resources/js/app.js` file
- Install `vite` as a devDependency

:::note
Depending on your project, you might need to install other vite plugins. Like `@vitejs/plugin-vue` for Vue.js projects or `@vitejs/plugin-react` for React projects. See below for more details.
:::

## Vite configuration

You should have a new `vite.config.ts` file at the root of your project with the following content : 

```ts
// title: vite.config.ts
import { defineConfig } from 'vite'
import adonisjs from '@adonisjs/vite/client'

export default defineConfig({
  plugins: [
    adonisjs({
      /**
       * Entrypoints of your application. Each entrypoint will
       * result in a separate bundle.
       */
      entrypoints: ['resources/js/app.js'],

      /**
       * Paths to watch and reload the browser on file change
       */
      reload: ['resources/views/**/*.edge'],
    }),
  ],
})
```

Similar to Webpack Encore, you can define multiple entry points. Each entry point will result in a separate bundle.

So make sure to add all your `Encore.addEntry()` calls to your new Vite configuration.

## Vite compatible imports

Vite only supports ES modules, so you will need to replace any `require()` statements with `import`.

## Add `@vite()` tag

Make sure you to replace your `entryPointStyles` and `entryPointScripts` calls with the `@vite()` tag :

```edge
// title: resources/views/welcome.edge
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AdonisJS - A fully featured web framework for Node.js</title>
  // highlight-start
  @vite(['resources/js/app.js'])
  // highlight-end
  @entryPointStyles('app')
  @entryPointScripts('app')
</head>
```

Note that with Webpack Encore you could specify a name for each entrypoint:

```ts
Encore.addEntry('app', './resources/js/app.js')
```

With Vite, you only need to specify the file name, and use the same name in your template :

```ts
// vite.config.ts
export default defineConfig({
  plugins: [
    adonisjs({
      entrypoints: ['resources/js/app.js'],
    }),
  ],
})
```

```edge
// title: resources/views/welcome.edge
@vite(['resources/js/app.js'])
```

## Typescript, CSS, Tailwind ..

Typescript, CSS, Postcss, Less, Sass, Tailwind: these tools should work out of the box. You don't need to configure anything. 

## Update environment variables

You will need to update environnement variables that should be exposed to the client code. Vite defaults to `VITE_` prefix, so you will need to update your `.env` file accordingly.

```diff
- MY_API_URL=http://localhost:3333
+ VITE_MY_API_URL=http://localhost:3333
```

You will also need to update your frontend code to use the new prefix :

```diff
- const api = new Ky.create({ prefixUrl: process.env.MY_API_URL })
+ const api = new Ky.create({ prefixUrl: import.meta.env.VITE_MY_API_URL })
```

## Vue

You need to install the `@vitejs/plugin-vue` plugin and add it to your `vite.config.ts` file.

```ts
// title: vite.config.ts
import { defineConfig } from 'vite'
import adonis from '@adonisjs/vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [
    adonis({
      entrypoints: ['resources/js/app.ts', 'resources/css/app.css'],
    }),
    // highlight-start
    vue()
    // highlight-end
  ],
})
```


## React

You will need to install the `@vitejs/plugin-react` plugin. 

```ts
// title: vite.config.ts
import { defineConfig } from 'vite'
import adonis from '@adonisjs/vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [
    adonis({
      entrypoints: ['resources/js/app.ts', 'resources/css/app.css'],
    }),
    // highlight-start
    react()
    // highlight-end
  ],
})
```

Also, for the HMR to work, you will need to add the following tag in your `edge` files :

```edge
// title: resources/views/welcome.edge
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AdonisJS - A fully featured web framework for Node.js</title>
  @vite()
  // highlight-start
  @viteReactRefresh()
  // highlight-end
</head>
```

## Uninstall webpack

You can remove webpack from your project by removing the following packages :

```bash
npm rm @symfony/webpack-encore webpack webpack-cli @babel/core @babel/preset-env @adonisjs/encore
rm webpack.config.js
```
