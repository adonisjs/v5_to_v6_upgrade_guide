# Upgrade packages

The **"upgrade packages"** patch will update official AdonisJS packages, remove the deprecated one's and install the new one's.

:::codegroup
```sh
// title: npm
npx @adonisjs/upgrade-kit upgrade-packages
```

```sh
// title: pnpm
pnpm exec @adonisjs/upgrade-kit upgrade-packages
```

```sh
// title: yarn
yarn dlx @adonisjs/upgrade-kit upgrade-packages
```
:::

Following are the steps performed by this patch.

## Packages upgraded

- Every `@adonisjs` and `@japa` packages will be upgraded to the latest version.
- `pino-pretty`

## Packages removed

- `@adonisjs/repl` - The REPL source code has been merged with the AdonisJS core. Therefore the package is no longer required.
- `source-map-support` - No longer need external packages for source maps. Node.js supports source maps as first class citizen now.

## Packages swapped

- `@adonisjs/view` swapped with `edge.js`. The `@adonisjs/view` package was a wrapper on top of `edge.js`. We decided to directly use the `edge.js` package.
- `phc-argon2` swapped with `argon2`. The `phc-argon2` was a wrapper on top of `argon2`. We decided to no longer use the wrapper and rely on the base implementation directly.
- `phc-bcrypt` swapped with `bcrypt`. The `phc-bcrypt` was a wrapper on top of `bcrypt`. We decided to no longer use the wrapper and rely on the base implementation directly.
- `@japa/preset-adonis` in swapped with `@japa/plugin-adonisjs`.
- `adonis-preset-ts` - The base configuration for TypeScript has been moved to `@adonisjs/tsconfig` package. Also, we will install `ts-node`, `@swc/core` packages to run TypeScript code without compiling it during development.

## Packages installed

- `@types/node` to have types for Node.js.
- `@adonisjs/validator` - The validator module from v5 is in legacy mode and we recommend using VineJS for new apps. However, for smoother upgrade experience, we install the `@adonisjs/validator` package, which brings the v5 validation module to v6 apps.
- `@adonisjs/cors` - The CORS source code has been moved to its own package. The patch will install and configure this package if there is an `config/cors.ts` file.
- `@adonisjs/static` - The source code to serve static files has been moved to its own package. The patch will install and configure this package if there is a `config/static.ts` file.

## Providers updated

Since AdonisJS packages exports Service providers, this patch will also update the `.adonisrc.json` file to use the new providers imports.

```diff
- "@adonisjs/core"
+ "@adonisjs/core/providers/app_provider"
+ "@adonisjs/core/providers/hash_provider"
```

```diff
- "@adonisjs/session"
+ "@adonisjs/session/session_provider"
```

```diff
- "@adonisjs/view"
+ "@adonisjs/core/providers/edge_provider"
```

```diff
- "@adonisjs/shield"
+ "@adonisjs/shield/shield_provider"
```

```diff
- "@adonisjs/lucid"
+ "@adonisjs/lucid/database_provider"
```

```diff
- "@adonisjs/redis"
+ "@adonisjs/redis/redis_provider"
```

```diff
- "@adonisjs/mail"
+ "@adonisjs/mail/mail_provider"
```

```diff
- "@adonisjs/ally"
+ "@adonisjs/ally/ally_provider"
```

```diff
- "@adonisjs/auth"
+ "@adonisjs/auth/auth_provider"
```

```diff
- "@adonisjs/repl"
+ {
+   "file": "@adonisjs/core/providers/repl_provider",
+   "environments": ["repl", "test"]
+ }
```

```diff
- "@japa/preset-adonis/TestsProvider"
```

- The `aceProviders` and `testProviders` have been removed. You can now register providers within the `providers` array and define the environment in which they should be imported.

## Commands updated

Since AdonisJS packages exports Ace commands, this patch will also update the `.adonisrc.json` file to use the new commands imports.

```diff
- "@adonisjs/repl/build/commands"
```

```diff
- "@adonisjs/lucid/build/commands"
+ "@adonisjs/lucid/commands"
```

In v6, there is no need to explicitly register commands part of your application codebase. They will be scanned automatically from the `./commands` directory. Also, we will delete the `./commands/index.ts` file as it is no longer needed.

```diff
- "./commands"
```
