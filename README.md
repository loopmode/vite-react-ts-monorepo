# vite typescript react monorepo

multiple apps and multiple shared packages.

## Adding app packages

```
yarn create @vitejs/app packages/app-appname --template react-ts
```

- Use the naming convention `app-appname`, replacing `appname` by the proper value. This is helpful when using the `--scope` and `--ignore` arguments in lerna commands (e.g. `lerna run build --ignore app-*`).
- You will be asked for a package name. Use the `@3gc` scope as prefix, e.g. `@3gc/app-appname`

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
