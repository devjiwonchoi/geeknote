You cannot destructure a stat function directly.

:x:

```ts
const { isDirectory, isFile } = await stat(path)
return { isDirectory, isFile }
```

:o:

```ts
const stats = await stat(path)
const { isDirectory, isFile } = stats

return { isDirectory: isDirectory.call(stats), isFile: isFile.call(stats) }
```

ref: https://github.com/nodejs/help/issues/3713#issuecomment-1026572462
