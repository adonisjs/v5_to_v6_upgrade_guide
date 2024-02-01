# Update Env validations code

The **"update env validation code"** patch will updates the `env.ts` file to be compatible with the new API.

:::codegroup

```sh
// title: npm
npx @adonisjs/upgrade-kit@latest upgrade-env-config
```

```sh
// title: pnpm
pnpm dlx @adonisjs/upgrade-kit@latest upgrade-env-config
```

:::

Following are the steps performed by this patch.

- Move `env.ts` to `start/env.ts` file.
- Rename `Env.rules` to `Env.create` and await the method call.

  ```diff
  - export default Env.rules({
  + export default await Env.create(new URL('../', import.meta.url), {
  ```

- Delete the `contracts/env.ts` file

That's all!
