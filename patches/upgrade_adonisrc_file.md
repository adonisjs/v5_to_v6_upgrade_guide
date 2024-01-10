# Upgrade Rc File

With V5, the `.adonisrc.json` file was used to a lot of things like defining aliases, commands, providers, etc.

With V6, the `.adonisrc.json` file is no longer used. Instead, we use the `adonisrc.ts` file to define all these things, for two main reasons:

- **Type safety**: A TypeScript file provides more type safety. The JSON does not complain if you use an invalid path to a provider or a preload file.
- **Better IDE integration**: In the AdonisJS codebase, service providers extend other modules by adding custom methods and properties. However, the types for these service providers were not picked up by the code editors because the import path of the service provider was inside a JSON file. Once we move to a TypeScript file, this problem disappears.

So this patch will migrate your `.adonisrc.json` file to `adonisrc.ts` file by keeping the options you've previously defined.

Make sure to always apply this patch as the last patch, since it will remove the `.adonisrc.json` file, and other patches may still need it.
