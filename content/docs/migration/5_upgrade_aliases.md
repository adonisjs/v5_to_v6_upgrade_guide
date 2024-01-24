# Upgrade aliases

The **"Upgrade aliases"** patch migrates your application from AdonisJS aliases to [Node.js sub-path imports](https://nodejs.org/dist/latest-v20.x/docs/api/packages.html#subpath-imports). Sub-path imports is a platform feature supported natively by Node.js and therefore we will embrace it with v6 applications.

:::codegroup

```sh
// title: npm
npx adonis-upgrade-kit upgrade-aliases
```

```sh
// title: pnpm
pnpm exec adonis-upgrade-kit upgrade-aliases
```

```sh
// title: yarn
yarn dlx adonis-upgrade-kitupgrade-aliases
```

:::

Following are the steps performed by this patch.

## Config files updated

- Scan `.adonisrc.json` file for existing aliases.
- Define a new set of aliases inside `package.json` file. Do note, the Node.js sub-paths import aliases must always start with a `#`.
- Define the same set of aliases inside `tsconfig.json` file as well.
- Remove aliases from the `.adonisrc.json` file.

## Source code update

- Finally, we will update the import statements in your codebase to use new aliases prefixed with a `#`.

## Un-effected areas

- The patch will not update any dynamic imports.
- The patch will not update magic strings based imports referenced within routes file or the events file.

**What is a magic string?**

In the following example, the `PostsController.index` is a magic string. Since, the value is a string internally translated to an import by AdonisJS.

```ts
import Route from '@ioc:Adonis/Core/Route'

Route.get('posts', 'PostsController.index')
```
