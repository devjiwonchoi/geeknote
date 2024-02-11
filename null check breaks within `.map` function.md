Issue:

```ts
import { resolve } from 'path'

const folders = ['alpha', 'bravo', 'charlie']

function resolveFolderPath(cwd?: string) {
  cwd ??= process.cwd()

  console.log(cwd) // string

  return folders.map((folder) =>
    resolve(
      cwd, // string | undefined
      folder
    )
  )
}
```

Wasn't sure why the type of cwd inside the `map` function still assumes as `undefined`.

I asked up the TS Discord channel, and was answered as:

```
It's a design limitation of TS, issue #9998.

You can work around it by saving the result to a const:
const base = cwd ?? process.cwd()
```

So I checked up issue [TS#9998](https://github.com/microsoft/TypeScript/issues/9998), seemed very interesting.

I came up with three options:

## Assign on Param

```ts
// ...
function resolveFolderPath(cwd: string = process.cwd()) {
  // ...
}
```

This does the same as `cwd ??= process.cwd()`, and is promising, but is not flexible enough.

## Non-Null Assertion

> See [https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#non-null-assertion-operator](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#non-null-assertion-operator)

```ts
// ...
function resolveFolderPath(cwd?: string) {
  cwd ??= process.cwd()
  return folders.map((folder) =>
    resolve(
      cwd!, // make sure this is not null
      folder
    )
  )
}
```

This seems not reliable on team working since people should know the context of the cwd has been `??=`

## Assign on different const

```ts
const cwd = maybeCwd ?? process.cwd()
```

This is the one the answeres recommended, and I also like this approach, seems reliable and flexible.
