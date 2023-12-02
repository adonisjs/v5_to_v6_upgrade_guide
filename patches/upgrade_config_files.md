# Upgrade Config Files

This patch will update the configuration files defined in `config/*.ts` to use the new configuration structure. Some of config files will be left untouched, since they are quite difficult to migrate automatically and can be manually updated easily in some minutes.

So make sure to read the [breaking changes](../other_breaking_changes.md) guide to know about the changes in the config files, and update them manually if required.

## Config files updated

- `config/app.ts`
- `config/bodyparser.ts`
- `config/cors.ts`
- `config/session.ts`
- `config/shield.ts`
- `config/static.ts`
- `config/database.ts`
- `config/redis.ts`
- `config/mail.ts`
- `config/ally.ts`

## Config files left untouched

- `config/auth.ts`

## Changes applied

There are three major changes applied to the config files :

- Migrate to the new `defineConfig` function
  Previously, there were several ways of defining the configuration for a module:
    - export const myConfig: MyConfigType = {}`
    - export default mailConfig({ /*...*/ })`
    - And many other variations.
  With V6, every package exports a `defineConfig` function. So what the patch does here is take the options you've previously defined, and pass them to this `defineConfig` function.
- Remove `contracts/*.ts` files associated with core packages. These files used to automatically infer certain types based on your configuration. With V6, these files are removed and types are defined directly in the configuration file. This patch therefore also applies this change.
- Migration to `config providers`. In V5, when you wanted to use one driver or another, you used strings. For example, in the `@adonisjs/mail` configuration, we was doing this :
  ```ts
  export default mailConfig({
    mailers: {
      smtp: {
        driver: 'smtp' // 👈👈 Specify the driver here
      }
    }
  })
  ```
  Keeping this structure with V5 was complicated. You can read more about it [here](https://github.com/adonisjs/road-to-v6/discussions/41).
  To continue with @adonisjs/mail example, with V6 we will instead do this :
  ```ts
  import { defineConfig, transports } from '@adonisjs/mail'

  export default defineConfig({
    mailers: {
      smtp: transports.smtp({
        // ...
      }),

      ses: transports.ses({
        // ...
      })
    }
  })
  ```

  So the patch will also apply this modification to most files where it is required.

If you ever have problems with this patch, feel free to consult the documentation for each package to see how to modify the new package configuration files. The web-starter-kit can also [be a good reference](https://github.com/adonisjs/web-starter-kit/tree/main/config)
