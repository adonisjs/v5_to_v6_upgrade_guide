# Upgrade module system

Upgrade module system from CommonJS to ESM. It involves the following steps.

- Set `type = module` inside `package.json` file.
- Update `tsconfig.json` to extend from `@adonisjs/tsconfig/app.tsconfig.json`.
- Disable the following rule temporarily. Otherwise all models will be red.
    ```json
    "strictPropertyInitialization": false,
    ```