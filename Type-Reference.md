# Type Inference
## base
In TypeScript, type inference will help provide type at some place where the type is not explicitly specified. Just like this:
```typescript
let x = 3;
```
The type of variable `x` will be inferred as `number`.

This kind of inference often occurs when initializing a variable or a member, setting the default value of parameter and deciding the returned value of a function.
<br/>

## Best Universal Type
When inferring the type from some expressions, compiler will infer the most suitable common type using these expressions. E.g.:
```typescript
let x = { 0, 1, null }
```
To infer the type of `x`, we must consider all the possible types of elements. There are two choices: `number`, `string`.  The computing generic type algorithm considers all candidate types and gives a type that is compatible with all candidate types.

## Context Type
Contextual categorization occurs when the type of an expression is relevant to its location.
for example:
```typescript
window.onmousedown = function(mouseEvent) {
    console.log(mouseEvent.button)
}
```
