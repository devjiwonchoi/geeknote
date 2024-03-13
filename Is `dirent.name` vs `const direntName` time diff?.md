Q: Is `dirent.name` vs `const direntName` time diff?

```ts
dirent.name
dirent.name
dirent.name
```

vs

```ts
const direntName = dirent.name

direntName
direntName
direntName
```

A: IMO assigning is slightly faster since it don't do object read operation
