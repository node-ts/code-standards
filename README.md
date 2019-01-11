# @node-ts/code-standards

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

1. Install into your project

```sh
npm i @node-ts/code-standards --save-dev
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
