# Upgrading from AdonisJS v5 to v6

> [!WARNING]
> This is a work in progress guide and not in usable state as of now. Feel free to read it, but do not attempt to migrate your apps until v6 is officially released.

## Breaking changes list other than applied patches

- [Breaking changes](./other_breaking_changes.md)

## Patches
The migration guide will come along with a migration CLI tool to apply patches to your application in order to make migration simpler and quicker.

Following is the initial list of patches.

- [Upgrade packages](./patches/upgrade_packages.md)
- [Update module system](./patches/update_module_system.md)
- [Update ESLint and Prettier setup](./patches/update_eslint_prettier_setup.md).
- [Migrate to Node.js sub-path imports](./patches/upgrade_aliases.md)
- [Migrate to newer AdonisJS imports](./patches/migrate_to_newer_imports.md)
- [Fix relative imports to use file extensions](./patches/fix_relative_imports.md)
- [Update Env validations code](./patches/update_env_validations_code.md)
- [Upgrade config files](./patches/upgrade_config_files.md.md)
- [Move to newer file structure for entry point files](./patches/upgrade_entrypoints.md)
- [Upgrade command options](./patches/upgrade_commands_options.md)
- [Upgrade AdonisRC file (Should be the last patch)](./patches/upgrade_adonisrc_file.md)

## Next steps

After applying all patches, you are ready to make last manual changes to your application. First, see the [breaking changes](./other_breaking_changes.md) list and make sure to fix them.

After that : 

- Migrate the community packages you are using. Some of them will probably have already been updated. So make sure to consult their respective documentation and changelog and migrate them.
- Check the status of your `config/*.ts` files. It's more than likely that the upgrade-kit wasn't able to migrate some of them correctly. You'll have to update them manually, but this should be very quick. Be sure to consult the V6 documentation to see how to configure the packages.
  - Move the logger configuration from `config/app.ts` to `config/logger.ts` using the new configuration system

    See [documentation](https://v6-alpha.adonisjs.com/docs/logger#configuration) and the [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/config/logger.ts) for an example.
- Update your Exception Handler using [the new API](https://v6-alpha.adonisjs.com/docs/exception-handling). 
- Update all your dynamic imports ( `await import('...')` ) using ESM-friendly imports (e.g. file extensions), and using subpath imports.

  Often, these imports are used in the commands you've created ( `commands/*.ts` ). Make sure you update them. Remember to use the new subpath-imports, so if for example you had `await import('App/Models/User')`, you can replace it with `await import('#app/Models/User')`.

- Update the `tests/bootstrap.ts` file using the new configuration system. Take the example of [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/tests/bootstrap.ts)
- ( Optional ) Migrate to the new Adonis 6 routing system. Remove all string-based routes and use [real controller imports](https://v6-alpha.adonisjs.com/docs/controllers#lazy-loading-controllers).
- Update the `start/kernel.ts` file using the [new API](https://github.com/adonisjs/web-starter-kit/blob/main/start/kernel.ts). 
- Add the middleware `container_bindings.ts` to the `app/middleware` folder and add it to the kernel. Again, use the [web-starter-kit](https://github.com/adonisjs/web-starter-kit/blob/main/app/middleware/container_bindings_middleware.ts) as an example.
- Migrate the authentication middleware.

## Sticking to v5

If you want to stick to v5, make sure to keep the following packages at their current version:

```jsonc
{
  /**
   * AdonisJS packages
   */
  "@adonisjs/ace": "^11.3.1",
  "@adonisjs/ally": "^4.1.5",
  "@adonisjs/application": "^5.3.0",
  "@adonisjs/assembler": "^5.9.6",
  "@adonisjs/attachment-lite": "^1.0.8",
  "@adonisjs/auth": "^8.2.3",
  "@adonisjs/bodyparser": "^8.1.9",
  "@adonisjs/bouncer": "^2.3.0",
  "@adonisjs/config": "^3.0.9",
  "@adonisjs/core": "^5.9.0",
  "@adonisjs/drive-gcs": "^1.1.2",
  "@adonisjs/drive-s3": "^1.3.3",
  "@adonisjs/drive": "^2.3.0",
  "@adonisjs/encryption": "^4.0.8",
  "@adonisjs/env": "^3.0.9",
  "@adonisjs/events": "^7.2.1",
  "@adonisjs/fold": "^8.2.0",
  "@adonisjs/hash": "^7.2.2",
  "@adonisjs/http-server": "^5.12.0",
  "@adonisjs/i18n": "^1.6.0",
  "@adonisjs/limiter": "^1.0.2",
  "@adonisjs/logger": "^4.1.5",
  "@adonisjs/lucid-slugify": "^2.2.1",
  "@adonisjs/lucid": "^18.4.2",
  "@adonisjs/mail": "^8.2.1",
  "@adonisjs/redis": "^7.3.4",
  "@adonisjs/repl": "^4.0.0",
  "@adonisjs/route-model-binding": "^1.0.1",
  "@adonisjs/session": "^6.4.0",
  "@adonisjs/shield": "^7.1.1",
  "@adonisjs/validator": "^12.6.0",
  "@adonisjs/view": "^6.2.0",

  /**
   * Japa Packages
   */
   "@japa/runner": "^2.5.1",
   "@japa/api-client": "1.4.2",
   "@japa/preset-adonis": "^1.2.0",
   "@japa/expect": "^2.0.2",
   "@japa/spec-reporter": "^1.3.3",
   "@japa/browser-client": "^1.2.0",
   "@japa/file-system": "^1.1.0",
   "@japa/snapshot": "1.0.1-3",
   "@japa/run-failed-tests": "^1.1.1"
}
```
