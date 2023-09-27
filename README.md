# Upgrading from AdonisJS v5 to v6

> [!WARNING]
> This is a work in progress guide and not in usable state as of now. Feel free to read it, but do not attempt to migrate your apps until v6 is officially released.

## Breaking changes list other than applied patches

- [Breaking changes](./other_breaking_changes.md)

## Patches
The migration guide will come along with a migration CLI tool to apply patches to your application in order to make migration simpler and quicker.

Following is the initial list of patches.

- [Upgrade packages](./patches/upgrade_packages.md)
- [Update module system](./patches/update_module_system.md)
- [Update ESLint and Prettier setup](./patches/update_eslint_prettier_setup.md).
- [Migrate to Node.js sub-path imports](./patches/upgrade_aliases.md)
- [Migrate to newer AdonisJS imports](./patches/migrate_to_newer_imports.md)
- [Fix relative imports to use file extensions](./patches/fix_relative_imports.md)
- [Update Env validations code](./patches/update_env_validations_code.md)
- [Update config files](./patches/update_config_files.md)
- [Move to newer file structure for entry point files](./patches/upgrade_entrypoints.md)
- [Upgrade command options](./patches/upgrade_commands_options.md)
- [Upgrade AdonisRC file (Should be the last patch)](./patches/upgrade_adonisrc_file.md)
