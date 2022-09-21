# Module
## Export
### Export declaration
Any declaration (such as a variable, function, class, type alias or interface) can be exported by adding the export keyword.

> Validation.ts 
```typescript
export interface StringValidator {
    isAcceptable(s: string): boolean;
}
```
> ZipCodeValidator.ts 
```typescript
export const numberRegexp = /^[0-9]+$/;
export class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string): boolean {
        return s.length == 5 && numberRegexp.test(s);
    }
}
```
### Export Statement
it is convenient to use the export statement. The example above can be modified like this:
```typescript
const numberRegexp = /^[0-9]+$/;
class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string): boolean {
        return s.length == 5 && numberRegexp.test(s);
    }
}

export { ZipCodeValidator };
export { ZipCodeValidator as mainValidator };
```
### Re-export
we may often extend other modules, and only export the content that is a part of that module. 

Re-exporting a function does not import that module or define a new local variable in the current module.

> ParseIntBasedZipCodeValidator.ts
```typescript
export class ParseIntBasedZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && parseInt(s).toString() === s;
    }
}

// 导出原先的验证器但做了重命名
export {ZipCodeValidator as RegExpBasedZipCodeValidator} from "./ZipCodeValidator";
```
<br/>

## Import
### Import a part of module
```typescript
import { ZipCodeValidator } from './ZipCodeValidator';
```
We can rename the imported content:
```typescript
import { ZipCodeValidator as ZCV } from './ZipCdeValidator';

let myValidator = new ZCV();
```
### Import the entire module into a variable and use it to access the exported part of the module
```typescript
import * as validator from "./ZipCodeValidator";
let myValidator = new validator.ZipCodeValidator();
```
### Import module with side effects
```typescript
import "./my-module.js"
```
<br/>

## Default Export
Each module can have a default export.
Default exports are marked with the default keyword; and a module can only have one default export.
A special import form is required to import the default export.

> JQuery.d.ts   
```typescript
declare let $: JQuery;
export default $;
```
> App.ts
```typescript
import $ from "JQuery";

$("button.continue").html( "Next Step..." );
```
### `export =` and `import = require()`
To support Common JS and AMD's `exports`, TypeScript provides the `export =` syntax.

The `export =` syntax defines an export object for a module.
The term object here refers to a class, interface, namespace, function or enumeration.

If you use `export =` to export a module, you must use the TypeScript specific syntax `import module = require("module")` to import the module.
<br/>


