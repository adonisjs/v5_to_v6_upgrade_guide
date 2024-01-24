# Next steps

Now that you have applied all patches, you are ready to make last manual changes to your application. First, see the [breaking changes](../other/other_breaking_changes.md) list and make sure to fix them.

After that, here are some things you have to do:

## Apply lint fixes

Since we have changed a lot of files using `upgrade-kit`, you should run your linter/formatter to fix the files.

```sh
npm run lint -- --fix
```

## Migrate the community packages

Migrate the community packages you are using. Some of them will probably have already been updated. So make sure to consult their respective documentation and changelog and migrate them.

## Check the status of your `config/*.ts` files

Check the state of your `config/*.ts` files. It's more than likely that the upgrade-kit wasn't able to migrate some of them correctly. You'll have to update some of them manually, but this should be very quick. Make sure to consult the V6 documentation to see how to configure the packages.

### Move the logger configuration

Earlier, we defined the logger configuration in `config/app.ts`. We now have moved it to `config/logger.ts` and changed its API.

See [documentation](https://v6-alpha.adonisjs.com/docs/logger#configuration) and the [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/config/logger.ts) for an example.

### Remove the profiler configuration

The profiler was never documented and is now removed. Remove the profiler configuration from `config/app.ts`.

## Update your Exception Handler

Update your Exception Handler using [the new API](https://v6-alpha.adonisjs.com/docs/exception-handling).

## Update dynamic imports

Update all your dynamic imports ( `await import('...')` ) using ESM-friendly imports (e.g. file extensions), and using subpath imports.

Often, these imports are used in the commands you've created ( `commands/*.ts` ). Make sure you update them.

Remember to use the new subpath-imports, so if for example you had `await import('App/Models/User')`, you can replace it with `await import('#app/Models/User')`.

## Update the `tests/bootstrap.ts` file

Update the `tests/bootstrap.ts` file using the new configuration system. Take the example of [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/tests/bootstrap.ts)

## ( Optional ) Migrate to the new Adonis 6 routing system

( Optional ) Migrate to the new Adonis 6 routing system. Remove all string-based routes and use [real controller imports](https://v6-alpha.adonisjs.com/docs/controllers#lazy-loading-controllers).

## ( Optional ) Migrate from Webpack Encore to Vite

Vite is now the recommended way to build your frontend assets. See the [migration guide](../other/vite_migration.md) to migrate from Webpack Encore to Vite.

## Update the `start/kernel.ts` file

Update the `start/kernel.ts` file using the [new API](https://github.com/adonisjs/web-starter-kit/blob/main/start/kernel.ts).

## Add container bindings middleware

Add the middleware `container_bindings.ts` to the `app/middleware` folder and also add it to the kernel. Again, use the [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/app/middleware/container_bindings_middleware.ts) as an example.

## Migrate the @adonisjs/auth middleware

## ( Optional ) Move contracts/_ files to config/_ files

With V5, we defined some typings on the `contracts` folder. We now have moved them directly in the associated `config` file. For example, the `contracts/hash.ts` file on V5 looks like this:

```ts
// title: contracts/hash,ts
import type { InferListFromConfig } from '@adonisjs/core/build/config'
import type hashConfig from '../config/hash'

declare module '@ioc:Adonis/Core/Hash' {
  interface HashersList extends InferListFromConfig<typeof hashConfig> {}
}
```

Now, with V6, you could only have the `config/hash.ts` file:

```ts
// title: config/hash.ts

import { defineConfig, drivers } from '@adonisjs/core/hash'

const hashConfig = defineConfig({
  default: 'scrypt',
  list: {
    scrypt: drivers.scrypt({
      cost: 16384,
      blockSize: 8,
      parallelization: 1,
      maxMemory: 33554432,
    }),
  },
})

export default hashConfig

/**
 * Inferring types for the list of hashers you have configured
 * in your application.
 */
declare module '@adonisjs/core/types' {
  export interface HashersList extends InferHashers<typeof hashConfig> {}
}
```

## Migrate edge breaking changes

AdonisJS 6 comes with a new version of Edge. See the [Edge migration guide](https://edgejs.dev/docs/changelog/upgrading-to-v6) and update your views accordingly.
