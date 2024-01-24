# Upgrade Command Options

This patch will migrate the commands options to the new structure.

:::codegroup

```sh
// title: npm
npx adonis-upgrade-kit upgrade-command-options
```

```sh
// title: pnpm
pnpm exec adonis-upgrade-kit upgrade-command-options
```

```sh
// title: yarn
yarn dlx adonis-upgrade-kitupgrade-command-options
```

:::

With V5, the command options were defined as follows:

```ts
export default class TestCommand extends BaseCommand {
  public static settings = {
    loadApp: false,
    stayAlive: false,
  }
}
```

With V6, the options are defined like this:

```ts
export default class TestCommand extends BaseCommand {
  static options: CommandOptions = {
    loadApp: false,
    staysAlive: false,
  }
}
```

The patch will therefore update the options of all commands defined in `commands` directory of your application.
