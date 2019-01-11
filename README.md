# @node-ts/code-standards

[![CircleCI](https://circleci.com/gh/node-ts/code-standards/tree/master.svg?style=svg)](https://circleci.com/gh/node-ts/code-standards/tree/master)

An opinionated set of linting and build configurations for typescript projects to build modern, maintainable TypeScript projects. 

## Linting

The linting rules aim to produce terse, neat and consistent TypeScript code. This is valuable in any repo that has more than one contributor, but single author projects may also find it useful.

Below shows a small example of the code produced with this style:

```typescript
// Enforce ES6 style imports
import { S3 } from 'aws-sdk' // single quotes, no semicolon on the end of lines

export class ObjectStorageService {

  constructor ( // double-space indents
    private readonly s3 = new S3()
  ) {
  }

  async upload (bucket: string, key: string, body: Buffer): Promise<void> { // enforce complete method signature
    const putObjectRequest: S3.Types.PutObjectRequest = { // prefer const
      Bucket: bucket,
      Key: key,
      Body: body
    }
    await this.s3.putObject(putObjectRequest).promise()
  }

}
// All files end with a new line (LF)
```

## TypeScript Configuration

TypeScript options have been set in `tsconfig.json` that target Node v8 and up. These options can be overridden for web projects that target browsers.

## Installation

1. Install into your project along with `tslint` and `typescript`

  ```sh
  npm i @node-ts/code-standards tslint typescript --save-dev
  ```

2. Copy `.editorconfig` into the root of your project. This will let your code editor confirm to many of the whitespacing rules automatically. See [Editor Config](https://editorconfig.org/) on setting up your code editor for the first time.

3. Create a `tslint.json` file in the root of your project with the following contents:

  ```json
  {
    "extends": "./node_modules/@node-ts/code-standards/tslint.json"
  }
  ```

4. Create a `tsconfig.json` file in the root of your project with the following contents. You may wish to extend this with further options

  ```json
  {
    "extends": "./node_modules/@node-ts/code-standards/tsconfig.json"
  }
  ```

5. Add the following to the `scripts` block in your project's `package.json`:

  ```json
  {
    "lint": "tslint --project tsconfig.json 'src/**/*.ts'",
    "lint:fix": "npm lint --fix"
  }
  ```

## IDE Configuration

IDE defaults for line spacing, whitespace etc can be set by placing an `.editorconfig` file (like the one in this package) into the root of your project. This is used by the [Editor Config](https://editorconfig.org/) plugin of your preferred browser to set such defaults for your project. 

This helps ensure any characters inserted by your editor conforms to the linting rules in this package.

## Overriding Rules

Individual rules can be overridden if they do not suit the particular project they're being imported into. This is common for web sites that need to transpile slightly differently to a regular node application.

### In tslint.json

Individual linting rules cna be overridden by specifying the updated rule in your local `tslint.json` file as such:

```json
{
  "extends": "./node_modules/@node-ts/code-standards/tslint.json",
  "rules": {
    // Override the default semicolon rule from "never" to "always"
    "semicolon": [true, "always"]
  }
}
```

### In tsconfig.json

Similar to linting, TypeScript configuration options can be overridden by specifying the updated values in the local `tsconfig.json` file.

For example:

```json
{
  "extends": "./node_modules/@node-ts/code-standards/tsconfig.json",
  "compilerOptions": {
    // Target es6 instead of es2017
    "target": "es6",
    "lib": ["es7"]
  }
}
```
