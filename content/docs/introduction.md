---
title: AdonisJS Migration Guide
summary: The migration guide for migrating from AdonisJS v5 to v6.
---

# Migration from AdonisJS v5 to v6

This guide is a **work in progress** to help you migrate your existing (v5) AdonisJS applications to v6. In order to make the migration process a little easier, we have created a `upgrade-kit` CLI tool that applies patches on an existing codebase.

## What's not ready?

The core of the framework is in great shape to create new applications and migrate over existing one's. However, not all the official and the community packages are compatible with v6 yet. This process might take another few weeks, as we and authors of other packages take out time to make their packages compatible with v6.

So, we recommend creating new apps with v6 right away. But wait a little (few more weeks) as the community picks up the new release and starting moving their packages to v6.

If you maintain a package and need help with migrating your package to be compatible with v6, then please open an issue with [adonisjs/v5_to_v6_upgrade_guide](https://github.com/adonisjs/v5_to_v6_upgrade_guide) repo.

### Official packages not yet compatible with v6

If your applications relies on the following packages, then please wait for another few weeks as we get these packages compatible with v6.

- [Attachment lite](https://github.com/adonisjs/attachment-lite)
- [Lucid Slugify](https://github.com/adonisjs/lucid-slugify)
- [Route model binding](https://github.com/adonisjs/route-model-binding)

## What's changed?

Apart of individual breaking changes across different packages, AdonisJS has moved to ESM and changed the internals of how its IoC container works. Both of these changes forces you update the imports of your application in almost every single file. For example:

- With ESM, you cannot import an `index.js` file its parent directory name. You will have to type the complete path.
- The imports must end with `.js` extension.
- Next, we remove the `@ioc` prefixed imports that were rewritten to IoC container lookup calls using a TypeScript compiler hook. So, these imports have to be replaced as well. [Learn more about it here](https://github.com/adonisjs/core/discussions/4184#:~:text=Changes%20to%20the%20import%20module%20names)
- Next, we remove the aliasing system of AdonisJS in favor of [Node.js subpath imports](https://nodejs.org/api/packages.html#subpath-imports). So instead of importing a model from `'App/Models/User'` path, you will have to import it from `'#models/User'` path.
- And finally, we use the Node.js [subpath exports](https://nodejs.org/api/packages.html#subpath-exports) to control the public modules API of your packages. This might lead to a value that was earlier exported from the main entrypoint now has been moved to a submodule.

All these changes combined does not break the internal APIs of existing modules. However, they do require manual labour to replace existing import paths with new import paths.

To make this journey simpler, you should use the [upgrade-kit CLI](./migration/upgrade_kit.md).
