# Other breaking changes

Here's a list of some breaking changes that are not covered by the patches provided by the upgrade-kit.

## Encryption package

- Zero breaking changes in the functional API

## Env

- Zero breaking changes in the functional API

## Config

- Remove undocumented `Config.merge` method.

## Logger

- Zero breaking changes in the functional API.
- Add support for multiple loggers.
- Upgrade to latest version of Pino.

## Events

- Remove deprecated `trap` and `trapAll` methods.
- Remove `namespace` method. Since we are opting for standard imports and not magic string based imports, the `namespace` method is no longer relevant.
- Add assertion methods like `assertNoneEmitted`, `assertNotEmitted`, `assertEmitted`.

## Bodyparser

- Remove `file.moveToDisk` method from the bodyparser source code. However, you can still use this method if you have `@adonisjs/drive` package installed.
- Removed `queryString` config option from raw bodyparser config block. The option was unused and hence has no breaking change at runtime, but will give static type error.
- Add support for `convertEmptyStringsToNull` for JSON parser as well.
- rename `whitelistedMethods` property to `allowedMethods`.

## Session

- Add `@adonisjs/session/session_middleware` to the middleware list in order to enable sessions. In v5, it was not needed.

## Encore

- No breaking changes

## Mail

- Zero breaking changes in the functional API.
- Add `alwaysTo` and `alwaysFrom` methods.
- Add `assertSent`, `assertNotSent`, `assertNoneSent` to write assertions during testing.

## Ally

- Zero breaking changes

## Hash

- Remove `Hash.isFaked` property since it serves no use case.
- The `Hash.extend` method signature has been changed.

  ```ts
  // Earlier
  Hash.extend('md5', (manager, mappingName, config) => {
    console.log(manager === Hash) // true
  })

  // Now
  Hash.extend('md5', (config) => {})
  ```

## Repl

- Zero functional breaking changes.
- Top-level imports do not work in REPL mode. It is a limitation of Node.js. However, you can use dynamic imports to import ES modules.

## Container

IoC Container has been rewritten from scratch and has received a massive API refactor. Since, the container is the foundation of the framework, there was no easy way to avoid this massive refactor.

With that said, the API changes will not impact the applications written in AdonisJS. The breaking changes will impact packages though.

Read the release notes for following releases.

- https://github.com/adonisjs/fold/releases/tag/v9.0.0-0

## Application

The application module has also received significant breaking changes. Please consult the following release notes.

- https://github.com/adonisjs/application/releases/tag/v6.0.0-0
- https://github.com/adonisjs/application/releases/tag/v6.3.0-0
- https://github.com/adonisjs/application/releases/tag/v6.8.0-0

## Http server

The Http server module has received significant changes. Please refer to the following release notes.

- https://github.com/adonisjs/http-server/releases/tag/v6.0.0-0
- https://github.com/adonisjs/http-server/releases/tag/v6.1.0-0
- https://github.com/adonisjs/http-server/releases/tag/v6.2.0-0

## Shield

- Remove DNS prefetching guard, since it is not a standard feature anymore. https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-DNS-Prefetch-Control

## i18n

- Rename `i18nManager.getSupportedLocale` to `i18nManager.getSupportedLocaleFor`.
- Rename `i18nManager.getFallbackLocale` to `i18nManager.getFallbackLocaleFor`.

## Redis

- No longer emit events using the AdonisJS global event emitter. These events were never documented.

## Ace

- https://github.com/adonisjs/ace/releases/tag/v12.0.0-0

## Lucid

- https://github.com/adonisjs/lucid/releases/tag/v19.0.0-0
