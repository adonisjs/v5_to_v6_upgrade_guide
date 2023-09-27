# Fix relative imports

Since, AdonisJS applications are written in TypeScript, which uses the `import/export` syntax while authoring the code, you are somewhat protected when it comes to the number of breaking changes while moving from CommonJS to ESM.

For example, you do not have to remove `module.exports`, `require` method calls.

However, you will have to rewrite the relative imports to match the following rules.

- All imports must specify the file extension. For example: Instead of writing `import User from './user'`, you will have to write `import User from './user.js'`.
- Even for TypeScript files, the file extension has to be `.js`.
- Index files are no longer imported by specifying the directory name.  For example:

    ```ts
    // ✅ Right way to import index.js file in ESM
    import InvoiceService from './services/invoice/index.js'

    // ❌ Works with CJS, but not ESM
    import InvoiceService from './services/invoice'
    ```
    
    
This patch will fix the relative imports in your application code and make them compatible with ESM.