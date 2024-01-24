# Migration guide

In this guide we gonna cover everything you need to know to migrate your AdonisJS v5 application to v6. First, let's introduce the `upgrade-kit` CLI tool.

## Upgrade Kit

The `Upgrade Kit` is a CLI tool that will help you migrate your AdonisJS v5 application to v6. It is built on using `ts-morph` so it is able to parse and modify your Typescript files directly. It probably won't be cover all the work needed to migrate your application, but it will take care of the most tedious and boring parts.

The upgrade kit has multiples patches that **must be applied in a specific order**. After executing a patch, **make sure to review the changes**, commit them, and then execute the next patch.

Some codemods are difficult to write perfectly, because we have to take into account a lot of different cases of writing code. So, it is possible that some patches will not be able to migrate your code correctly. In this case, you can always fix the code manually, by following the the instructions for each patch just below. Otherwise, feel free to also open an issue on the [Upgrade Kit repository](https://github.com/adonisjs/upgrade-kit) and we will try to fix it as soon as possible.

## Installation

First you need to install the upgrade kit globally :

:::codegroup

```sh
// title: npm
npm i -g @adonisjs/upgrade-kit
```

```sh
// title: pnpm
pnpm i -g @adonisjs/upgrade-kit
```

```sh
// title: yarn
yarn global add @adonisjs/upgrade-kit
```

:::

## Usage

To use the upgrade kit, you can run the following command.

:::codegroup

```sh
// title: npm
npx adonis-upgrade-kit {patchName} --path=/path/to/your/project
```

```sh
// title: pnpm
pnpm exec adonis-upgrade-kit {patchName} --path=/path/to/your/project
```

```sh
// title: yarn
yarn dlx adonis-upgrade-kit{patchName} --path=/path/to/your/project
```

:::

- `{patchName}` should be replaced by the name of one of the patches listed below
- `--path` should be replaced by the path to your project. This is optional, if not provided, the current directory will be used.

## Patches

Here is the list of patches. They are ordered by the order in which they should be applied.

### Upgrade packages

Patch name : `upgrade-packages`

Will update official AdonisJS packages to their new version.

See [Upgrade packages](./1_upgrade_packages.md)

### Update module system

Patch name : `upgrade-module-system`

Move from CommonJS to ESM by updating `tsconfig.json` and `package.json`.

See [Update module system](./2_update_module_system.md)

### Update ESLint and Prettier setup

Patch name : `upgrade-eslint-prettier`

Update ESLint and Prettier setup.

See [Update ESLint and Prettier setup](./3_update_eslint_prettier_setup.md)

### Update Env validations code

Patch name : `upgrade-env-config`

Move to the new Env API.

See [Update Env validations code](./4_update_env_validations_code.md)

### Migrate to Node.js sub-path imports

Patch name : `upgrade-aliases`

Move from the old aliases to Node.js subpaths imports.

See [Migrate to Node.js sub-path imports](./5_upgrade_aliases.md)

### Migrate to newer AdonisJS imports

Patch name : `migrate-ioc-imports`

Move from `@ioc:` imports to standard imports.

See [Migrate to newer AdonisJS imports](./6_migrate_to_newer_imports.md)

### Fix relative imports to use file extensions

Patch name : `fix-relative-imports`

Fix relative imports and add `.js` extensions ( needed for ESM ).

See [Fix relative imports to use file extensions](./7_fix_relative_imports.md)

### Move to newer file structure for entry point files

Patch name : `upgrade-entrypoints`

Add new entrypoints needed for Adonis.js v6

See [Move to newer file structure for entry point files](./8_upgrade_entrypoints.md)

### Upgrade config files

Patch name : `upgrade-config-files`

Update `config/*.ts` files to use their new APIs.

See [Upgrade config files](./9_upgrade_config_files.md)

### Upgrade command options

Patch name : `upgrade-command-options`

Update the command `options` property to use the new API.

See [Upgrade command options](./10_upgrade_commands_options.md)

### Upgrade AdonisRC file (Should be the last patch)

Patch name : `upgrade-rcfile`

Move from `.adonisrc.json` to `adonisrc.ts`.

See [Upgrade AdonisRC file](./11_upgrade_adonisrc_file.md)

## Next steps

After applying all patches, you are ready to make last manual changes.
