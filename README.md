# Reproducing the issue

* Do `npm i` in `upstream/` -- it should succeed
* Do `npx tsc` in `upstream/` -- it should succeed
* Do `npm i` in `downstream/` -- it should succeed
* Do `npm run test` in `downstream/` -- it should succeed and give both trace resolution and file list output

# Examining the output

The resolution tracing should at some point contain lines similar to the following:

```
Found 'package.json' at '.../ts-reference/downstream/node_modules/upstream/package.json'.
Using 'exports' subpath '.' with target './dist/specific_name.js'.
```

This shows that the package.json of the upstream package is being meaningfully used to determine behavior. Otherwise, the specific name of the .js file to use for the "upstream" module couldn't be determined.

The file list output should have lines related `typescript/lib`, `ts-reference/upstream/dist/specific_name.d.ts` (i.e. the upstream types), and `ts-reference/downstream/src/index.ts` (i.e. the TypeScript being transpiled), but should not contain the upstream package.json. This is unexpected behavior, as the upstream package.json was clearly used in determining `tsc`'s behavior. 