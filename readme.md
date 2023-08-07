# esmock-repro-cjs

Repro for https://github.com/iambumblehead/esmock/issues/218. Run `npm test` to reproduce error.

## Issue

Mocking globally using TypeScript with the `import` key fails if a module has a CJS import:

```ts
// src/test.ts
import esmock from "esmock";

const logs: string[] = [];

await esmock("./index.ts", import.meta.url, {}, {
	import: {
		console: { log: (...args: string[]) => logs.push(...args) },
	},
});

console.log("logs:", ...logs);
```

Error message:

```
file:///.../node_modules/camelcase-keys/index.js?esmkgdefs=file:///import?esmkTreeId=1&esmkModuleId=import&isfound=false&isesm=false&exportNames=console:1
import {console} from 'file:///import?esmkTreeId=1&esmkModuleId=import&isfound=false&isesm=false&exportNames=console';import mapObject from 'map-obj';
                                                                                                                             ^^^^^^^^^
SyntaxError: The requested module 'map-obj' does not provide an export named 'default'
    at ModuleJob._instantiate (node:internal/modules/esm/module_job:124:21)
    at async ModuleJob.run (node:internal/modules/esm/module_job:190:5)
```
