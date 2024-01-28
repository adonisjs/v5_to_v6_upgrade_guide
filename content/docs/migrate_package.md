# Migrate a package to v6

If you are the maintainer of an AdonisJS package, this guide is definitely for you. We will try to cover the major changes that you need to make to your package to make it compatible with AdonisJS v6.

## Update Tooling

Make sure to keep an eye on [`pkg-starter-kit`](https://github.com/adonisjs/pkg-starter-kit) repo for the tooling configuration as inspiration for the following steps.

### ESM

Just add `type: module` to your `package.json`

```json
{
  "type": "module"
}
```

### `mrm`

We used to use `mrm` to update configuration files. We've decided to stop using `mrm` as it's no longer really maintained and was causing some problems. So make sure you remove all references to `mrm` from your `package.json`.

### Eslint / Prettier

If you were using the default configuration, you would probably be using `eslint-plugin-adonis` to lint the codebase of your package. From now, we recommend using `@adonisjs/eslint-config` and `@adonisjs/prettier-config`. Make sure you install them as devDependencies.

```sh
pnpm add -D @adonisjs/eslint-config @adonisjs/prettier-config
```

Then add this to your `package.json` file

```json
{
  "eslintConfig": {
    "extends": "@adonisjs/eslint-config/package"
  },
  "prettier": "@adonisjs/prettier-config"
}
```

Make sure you also remove `.eslintrc.json` and `.eslintignore` from your project if they exist.

### Typescript

Install the following packages:

```sh
pnpm add -D ts-node @swc/core @adonisjs/tsconfig
```

You can also delete `@adonisjs/require-ts` from your `package.json`.

Then update your `tsconfig.json` like this:

```json
{
  "extends": "@adonisjs/tsconfig/tsconfig.package.json",
  "compilerOptions": {
    "rootDir": "./",
    "outDir": "./build"
  }
}
```

Then create a `tsnode.esm.js` file at the root of your project with the following contents:

```js
/*
|--------------------------------------------------------------------------
| TS-Node ESM hook
|--------------------------------------------------------------------------
|
| Importing this file before any other file will allow you to run TypeScript
| code directly using TS-Node + SWC. For example
|
| node --import="./tsnode.esm.js" bin/test.ts
| node --import="./tsnode.esm.js" index.ts
|
|
| Why not use "--loader=ts-node/esm"?
| Because, loaders have been deprecated.
*/

import { register } from 'node:module'
register('ts-node/esm', import.meta.url)
```

Then make sure you run your scripts/tests/code with this script rather than `@adonisjs/require-ts`.

For example, to launch your tests, you can define this script in your `package.json` :

```json
{
  "scripts": {
    "test": "node --import=./tsnode.esm.js --enable-source-maps bin/test.ts"
  }
}
```

Another topic, but note the `--enable-source-maps` which allows you to have stack traces with the correct line numbers. This is a [ts-node bug](https://github.com/TypeStrong/ts-node/issues/2053)

### Updating dependencies

Make sure you update all dependencies to @adonis and @japa in your `package.json`. You probably also want to add `@adonisjs/core` as peerDependencies :

```json
{
  "peerDependencies": {
    "@adonisjs/core": "^6.2.0"
  }
}
```

## Package configuration

Previously, we used an `instructions.ts` to allow the user to configure the package at installation via `node ace configure my-package`. We've changed the way it works.

You can delete the `adonisjs` field from your `package.json` which is no longer in use, also as the `instructions.ts` and `instructions.md` files. ( Keep instruction.ts though, as part of it can be re-used )

Now, to have this configuration logic, you'll need to define a `configure` method, and re-export it from the entrypoint ( `index.ts` ) of your package like this :

- https://github.com/adonisjs/pkg-starter-kit/blob/main/index.ts
- https://github.com/adonisjs/pkg-starter-kit/blob/main/configure.ts

This `configure` method will be called by Adonis when the user launches `node ace configure my-package`.
One point to note is that `@adonisjs/sink` is deprecated. Instead, the `configure` method receives a `ConfigureCommand` object as a parameter, which does the same thing as `@adonisjs/sink` but with a more powerful API.

There's a lot to say about the new API, so I recommend that you:

- Read the documentation here: https://github.com/adonisjs/assembler?tab=readme-ov-file#codemods
- take some inspiration from the `configure` in the official packages. For example, the `configure` command in `@adonisjs/mail`: https://github.com/adonisjs/mail/blob/develop/configure.ts

### Stubs/templates

Before, we used to use a `templates` folder in which we put `.txt` files which were copied with `sink` via `instructions.ts` file. Now you need to :

- Create a `stubs` folder at the root of your project.
- Add a `main.ts` to this folder containing this code:

  ```ts
  import { dirname } from 'node:path'
  import { fileURLToPath } from 'node:url'

  /**
   * Path to the root directory where the stubs are stored. We use
   * this path within commands and the configure hook
   */
  export const stubsRoot = dirname(fileURLToPath(import.meta.url))
  ```

- Then you can create a `.stub` file in this folder. Let's imagine a `config.stub` that should be copied into the `config` folder of the user's project. You can do it like this:

  ```ts
  {
    {
      {
        exports({ to: app.configPath('my_package_config.ts') })
      }
    }
  }

  import { defineConfig } from 'my-package'

  export default defineConfig({
    // ...
    myProperty: '{{ myProperty }}',
  })
  ```

  You can read more about stub syntax here: https://docs.adonisjs.com/guides/scaffolding#using-stubs

- Next, make sure you copy your stub files to the final build folder at build time. You can do this as follows:

  ```json
  {
    "scripts": {
      "copy:templates": "copyfiles \"stubs/**/*.stub\" build",
      "build": "tsc",
      "postbuild": "npm run copy:templates"
    }
  }
  ```

And that's it. Now, in your `configure.ts` file, you can do like this to publish the config file in the user's project:

```ts
import { stubsRoot } from './stubs/main.js'
import type Configure from '@adonisjs/core/commands/configure'

export async function configure(command: Configure) {
  const codemods = await command.createCodemods()

  await codemods.makeUsingStub(stubsRoot, 'config.stub', {
    myProperty: 'foo', // Will be passed to the stub template
  })
}
```

## Providers / Services

The major change here will be the way dependencies are injected and resolved via the IoC container. Be sure to read the @adonisjs/fold changelog before continuing. There's a specific section for package maintainers:

- https://github.com/adonisjs/fold/releases/tag/v9.0.0-0

Make sure you also read the following documentation:

- https://docs.adonisjs.com/guides/ioc-container#container-bindings
- https://docs.adonisjs.com/guides/container-services

If you ever maintain a package that works with a driver system, I also invite you to read this post on `Config Providers` which will be essential for you:

- https://github.com/adonisjs/road-to-v6/discussions/41

You should know everything you need to know to migrate your provider.

## `adonis-typings`

If you've been following the previous section, you'll notice that we no longer use `@ioc` imports at all.
The user will import your functions/services/classes directly from your package. **As a result, the `adonis-typings` folder and its contents are no longer needed**, which is a real time-saver for you, and also much simpler to maintain, as there's no additional place to update just for having correct typings.

## Commands

In case your package provides some commands to the user's application, you have a few modifications to make.

Previously:

- You defined your commands in `adonisjs.commands` of your `package.json`. As a reminder, the `adonisjs` property in `package.json` is now deprecated. You can delete it.
- You had a `commands/index.ts` which exported an array of commands. This file is no longer used. You can delete it.

With V6, your package will have to expose a `commands` export path as follows in the `package.json` :

```ts
{
  "exports": {
    ".": "./build/index.js",
    "./commands": "./build/commands/main.js",
  }
}
```

Then, in your `postbuild`, you now need to use `adonis-kit`, which is a CLI tool exposed by `@adonisjs/core` that will index your commands. You can do it like this:

```json
{
  "scripts": {
    "copy:templates": "copyfiles \"stubs/**/*.stub\" build",
    "build": "tsc",
    "postbuild": "npm run copy:templates && npm run index:commands",
    "index:commands": "adonis-kit index build/commands"
  }
}
```

And that's it.

## Tests

Make sure you upgrade to Japa V3. Read the migration guide here: https://japa.dev/docs/uprade-guide

Otherwise, I invite you to take a look at the tests of different official package to see how they are now written. Here are a few key points:

- Most packages now export `factories` which allow you to test your code more easily when you need an instance of something. You can access them via `import { xxxFactory } from '@adonisjs/pkg'`.
- In the past, when we needed to start an Adonis application for testing, you'd use big helpers like `setupApp` in which you had to make a bunch of boilerplates, create files on the filesystem... This is no longer necessary.

  - Example on `@adonisjs/mail` for version 5: https://github.com/adonisjs/mail/blob/68addd6bc952b7a4d545459455627652ac21e908/test-helpers/index.ts#L16
  - Example on `@adonisjs/mail` for version 6: https://github.com/adonisjs/mail/blob/develop/tests/integration/mail_provider.spec.ts#L25

  So you can create an application via the IgnitorFactory, pass options for each of the different packages, pass adonisrc file options... Much simpler.
