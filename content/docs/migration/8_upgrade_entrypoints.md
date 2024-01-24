# Upgrade Entrypoints

The **"Upgrade entrypoints"** patch migrates your application entrypoints to use the new ones needed for running AdonisJS v6.

:::codegroup

```sh
// title: npm
npx @adonisjs/upgrade-kit upgrade-entrypoints
```

```sh
// title: pnpm
pnpm exec @adonisjs/upgrade-kit upgrade-entrypoints
```

```sh
// title: yarn
yarn dlx @adonisjs/upgrade-kit upgrade-entrypoints
```

:::

- We will add 3 new entrypoints files, `bin/console.ts`, `bin/test.ts` and `bin/server.ts`. You can find the source code for these files in the [`web-starter-kit`](https://github.com/adonisjs/web-starter-kit/tree/main/bin) repository.
- We remove the old entrypoint files, `server.ts` and `test.ts`.
- We also remove some of old files which are no longer required : `ace`, `ace-manifest.json` and `commands/index.ts`.
- We add a new `ace.js` file. Code for this file can be found [here](https://github.com/adonisjs/web-starter-kit/blob/main/ace.js).
