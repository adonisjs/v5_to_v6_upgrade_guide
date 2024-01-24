# Update module system

The **"update module system"** patch moves your application from CommonJS to ESM.

:::codegroup

```sh
// title: npm
npx adonis-upgrade-kit upgrade-module-system
```

```sh
// title: pnpm
pnpm exec adonis-upgrade-kit upgrade-module-system
```

```sh
// title: yarn
yarn dlx adonis-upgrade-kitupgrade-module-system
```

:::

Following are the steps performed by this patch.

- Define `type = module` inside the `package.json` file.
- Make `tsconfig.json` file extend the new base config from `@adonisjs/tsconfig` package.
- Remove unnecessary known types from the `compilerOptions.types` array.

  ```json
  {
    "compilerOptions": {
      "types": [
        "@adonisjs/core",
        "@adonisjs/repl",
        "@adonisjs/session",
        "@adonisjs/view",
        "@adonisjs/shield",
        "@adonisjs/lucid",
        "@adonisjs/auth",
        "@adonisjs/lucid-slugify",
        "@adonisjs/drive-s3",
        "@adonisjs/attachment-lite",
        "@japa/preset-adonis/build/adonis-typings"
      ]
    }
  }
  ```
