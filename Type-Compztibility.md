# Type Compatibility
## Introduction
Type compatibility in TypeScript is based on structural subtypes.
A structure type is a way of describing a type using only its members, which is opposite to nominal types.
For Example:
```typescript
interface Named {
    name: string;
}

class Person {
    name: string;
}

let p: Named;

// okay, because of structural typing;
p = new Person();
```
The basic rule of TypeScript's structured type system is that if `x` is to be compatible with `y`, then `y` has at least the same properties as `x`.

For example:
```typescript
interface Named {
    name: string;
}

let x: Named;
// y's inferred type is { name: 'John'; age: 42 };
let y = { name: 'John', age: 42 };
x = y
```
Here, to check whether `y` is able to be assigned to `x`, compiler checks every property in `x`, Looking for the certain properties that is in `y`.

In this example, `y` must include the member that its key is `name` an the type of it is `string`. Obviously, `y` satisfy the demand.

It is the same when checking the parameters of a function:
```typescript
function greet(n: Named) {
    console.log('Hello, ' + n.name);
}
greet(y); // ok;
```
Note that `y` has an additional `location` property, but this does not raise an error.
Only members of the target type (Named in this case) are checked for compatibility.
<br/>

## Compare Two Function
Relatively speaking, it is easy to understand when comparing primitive types and object types. The problem is how to judge whether the two functions are compatible. Let's start with two simple functions, which are only slightly different in the parameter list:
```typescript
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // ok
x = y; // error();
```
To check whether `x` can be able to be assigned to `y`, we should check their parameter list first. Every parameter in `x` must be able to be found in `y`.

You may wonder why it is allowed to ignore parameters, as in the example `y = x`. The reason is that it is common in javascript to ignore additional parameters. For example, `Array#forEach` passes three parameters to the callback function: array elements, indexes, and the entire array. Nevertheless, it is also useful to pass in a callback function that uses only the first parameter.

Let's look at how to cope with the returned type:
```typescript
let x = () => ({ name: "Alice" })
let y = () => ( { name: "Alice", location: "Seattle" })

x = y; // ok;
y = x; // Error
```
The type system forces that the return type of the source function must be a subtype of the return type of the target function.

