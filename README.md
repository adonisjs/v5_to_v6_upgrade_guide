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
