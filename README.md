# Creating NPM packages: The Complete Guide

## Creating first package

### Generating project

```sh
pnpm init
```

```json
{
  "name": "greeting_package",
  "version": "1.0.0",
  "description": "This package is for greeting a person during the day",
  "repository": {
    "type": "git",
    "url": "https://github.com/codeef/greeting_package"
  },
  "main": "index.js",
  "author": "codeef",
  "license": "MIT"
}
```

### Name property

- scoped
  - private by default
  - public with _npm publish --access public_
- unscoped

### Module systems

- CJS (CommonJS)
- AMD (Async module definition)
- UMD (Universal module definition): CJS+AMD
- ESM (Ecma script module)

### Setting tsconfig

```sh
pnpm add -D typescript
```

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationDir": "lib/types",
    "target": "es6",
    "moduleResolution": "node"
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "lib"]
}
```

### Installing Rollup

```sh
pnpm add -D rollup rollup-plugin-typescript2 rollup-plugin-delete
```

### Rollup configuration

```js
import typescript from "rollup-plugin-typescript2";
import del from "rollup-plugin-delete";

export default {
  input: "src/index.ts",
  output: [
    {
      file: "lib/index.cjs",
      format: "cjs",
    },
    {
      file: "lib/index.esm.js",
      format: "esm",
    },
  ],
  plugins: [
    del({ targets: ["lib/*"] }),
    typescript({ useTsconfigDeclarationDir: true }),
  ],
};
```

```json
{
  "name": "greeting_package",
  "version": "1.0.0",
  "description": "This package is for greeting a person during the day",
  "repository": {
    "type": "git",
    "url": "https://github.com/codeef/greeting_package"
  },
  "type": "module",
  "main": "index.js",
  "scripts": {
    "build": "rollup -c"
  },
  "author": "codeef",
  "license": "MIT",
  "devDependencies": {
    "rollup": "^4.54.0",
    "rollup-plugin-delete": "^3.0.2",
    "rollup-plugin-typescript2": "^0.36.0",
    "typescript": "^5.9.3"
  }
}
```
