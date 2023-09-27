# Migrate AdonisJS imports

The **"migrate AdonisJS imports"** patch removes the existing `@ioc` prefixed imports from your application source in favor of new standard ESM imports.

## History of `@ioc` prefixed imports

AdonisJS uses the IoC Container to distribute pre-configured services like the `router`, `mailer`, `emitter`, and so on. For example, if you want an instance of the mailer, you can write the following code.

```ts
const Mail = container.make('Adonis/Addons/Mail')
```

Behind the scenes, we will read the config from the `config/mail.ts` file, create an instance of the `Mailer` class, and return a singleton instance.

In v5, we also decided to offer an import syntax for the `container.make` function call so you have a unified syntax for importing standard modules and resolving bindings from the container. Following is an example of the same.

```ts
import Mail from '@ioc:Adonis/Addons/Mail'
```

Behind the scenes, we use the TypeScript compiler API to rewrite the import as a function call, as shown in the first example. This approach has many drawbacks.

- The `@ioc` import feels alien since it is particular to AdonisJS.
- We must rely on the TypeScript official compiler API to rewrite the import. Hence, we cannot use faster tools like SWC or ESBuild.
- We have to separately define the `@ioc: Adonis/Addons/Mail` types because the TypeScript compiler cannot resolve this module using its filesystem resolution logic.


Cut to v6. We are eliminating all this complexity and homegrown style of `@ioc` imports. This patch will rewrite the imports for you, so you do not have to replace them.

## What imports have been written.

Share link to imports map we have got.