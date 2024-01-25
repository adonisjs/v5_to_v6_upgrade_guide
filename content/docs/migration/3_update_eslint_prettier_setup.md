# Update ESLint and Prettier setup

The **"Update ESLint and Prettier setup"** patch will update your existing ESLint config and Prettier config files to use the new base configuration shipped with v6 apps.

:::warning
If you are using a custom setup for both ESLint and Prettier, then you can skip patch. However, then you will have manually update `eslint` and its plugins and ensure they work smoothly with an ESM application.
:::

:::codegroup

```sh
// title: npm
npx @adonisjs/upgrade-kit@latest upgrade-eslint-prettier
```

```sh
// title: pnpm
pnpm dlx @adonisjs/upgrade-kit@latest upgrade-eslint-prettier
```

```sh
// title: yarn
yarn dlx adonis-upgrade-kit@latest upgrade-eslint-prettier
```

:::

Following are the steps performed by this patch.

## Config files removed

- Remove `eslintrc.json` and `.eslintignore` files. The new config will be inline within the `package.json` file.
- Remove `.prettierrc` file. The new config will be inline within the `package.json` file.

## Packages upgraded

- `eslint`
- `prettier`

## Packages removed

The following packages have been removed in favour of a single base configuration package `@adonisjs/eslint-config`.

- `eslint-config-prettier`
- `eslint-plugin-adonis`
- `eslint-plugin-prettier`

## Newly added packages

- `@adonisjs/eslint-config` - Contains base rules for ESLint and ensure it works great with TypeScript, ESM, and Prettier.
- `@adonisjs/prettier-config` - Contains base configuration for prettier rules we are using with v6 apps.

## Temporary rules

We disable the following ESLint rules temporarily to make migration from `v5 -> v6` smoother.

```json
"rules": {
  "@typescript-eslint/explicit-member-accessibility": "off",
  "unicorn/filename-case": "off",
  "@typescript-eslint/no-shadow": "off"
}
```

- `@typescript-eslint/explicit-member-accessibility`: In v6 we no longer use TypeScript class property modifiers like `public`, or `private`. Instead we use JavaScript native private properties prefixed with a `#`.

  \
   However, removing property modifiers from an entire codebase can be a tedious task and therefore we turn off this rule temporarily. If needed, you can turn it back on after the migration is completed and your app is in working state.

- `unicorn/filename-case`: In v6, we opted for a `snake_case` naming convention for the naming folders and files. Again, to make migration smooth, we turn off the ESLint rule.

- `@typescript-eslint/no-shadow` is a newly added rule and might force you to refactor your application codebase. We turn off this rule to make migration process smooth.
