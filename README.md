# vite typescript react monorepo

multiple apps and multiple shared packages.

## @project scope

Either use `@project` as scope for all packages, as those packages will remain private anyways. This helps you quickly check on your symlinked packages in `node_modules/@project/*`, all in one place.

Or alternatively use some other useful scope, like your project name (e.g. `@mystuff`). In this case, run an initial search and replace for `@project` -> `@yourscope`.

When describing conventions later on, when we refer to the scope `@project`, we basically mean `@whatever-scope-you-used.`

## Adding app packages

You can use the official way of creating vite apps:

```
yarn create @vitejs/app packages/app-appname --template react-ts
```

### Suggested naming convention

- Use the naming convention `app-appname`, replacing `appname` by the proper value. This is helpful to distinguish apps from regular/shared packages when using the `--scope` and `--ignore` arguments in lerna commands (e.g. `lerna run build --scope app-*`).
- You will be asked for a package name. Use the `@project` scope as prefix, e.g. `@project/app-appname`

## Typescript configuration

### tsconfig for app packages

Extend your `tsconfig.json` from `../../tsconfig.app.json`, and include other workspace packages that you want working auto-imports to in VSCode.

Example app tsconfig.json:

```
{
  "extends": "../../tsconfig.app.json",
  "include": ["./src", "../core/lib"]
}
```

In this example, VSCode will give you nice auto-imports from this app to anything in the `core` package.

### tsconfig for non-app packages

Regular packages should extend from `../../tsconfig.lib.json`. Also, the packages themselves _must_ be registered in `tsconfig.app.json` so that they are available in app packages.

Example non-app tsconfig.json:

```
{
  "extends": "../../tsconfig.lib.json",
  "include": ["src"],
  "compilerOptions": {
    "baseUrl": ".",
    "rootDir": "src",
    "outDir": "lib"
  }
}
```

If you want auto-imports working to other packages, same goes as for apps: You should put paths to those other packages into the `include` array.

Also note the entry in `tsconfig.app.json` in the root:
```
    ...
   "paths": {
      "baseUrl": ["."],     
      "@project/core/*": ["./packages/core/*"]
    }
    ...
```
Without this, you won't be able to consume the package in any app (as they extend from `tsconfig.app.json`)
