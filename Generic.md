# Generic
## Begin of Generic
Let's create first example using Generic: `identity` function, which will return nay value passed in. 

if we do not use generic, this function will like this:
```typescript
function identity(arg: number): number {
    return arg;
}
```
or
```typescript
function identity(arg: any): any {
    return arg;
}
```
Using `any` type will cause this function to accept `any` type of arg parameter, which will loe some information: the type passed in and return type should be the same. If we pass a number, we only know that any type will be returned.

Therefore, we need a way to make the return type is the same as the parameter type. Here, we use type variable, which is a kid of special variable that only is used to represent type but not value.
```typescript
function identity<T>(arg: T): T {
    return arg;
}
```
After we define the generic function, we can use it in two ways: the first is passe all the parameters, including the type parameter:
```typescript
let output = identity<string>("myString");
```
Here, we specify `T` as `string` type, and as a parameter passed to the function.

The second way is more common. Use type reference ------ that is compiler will automatically help us specify `T` according to the parameter passed in.
```typescript
let output = identity("myString");
```
<br/>

## Using Generic Variable
When using generic to create the generic function like `identity`, compiler requires you must correctly use this common type in function content. That is, you must treat these parameters as any or all types.

look at the previous example:
```typescript
function identity<T>(arg: T): T {
    return arg;
}  
```
If we want to print the length of the `arg` at the same time, we are likely to do like this:
```typescript
function identity<T>(arg: T): T {
    console.log(arg.length); // error: T doesn't have .length;
    return arg;
}
```
the correct way to do this is:
```typescript
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length)
    return arg;
}
```
<br/>

## Generic Type
in last passage, we create `identity` common function, which is suitable to various types.

In this part, we will research the type of function itself, and how to create a generic interface.

The type of a generic function is no different from the type of a non-generic function, except that there is a type argument at the front, like a function declaration:
```typescript
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <T>(arg: T): T = identity;
```
We can also use different generic parameter names, as long as the number and usage correspond.
```typescript
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <U>(arg: U): U = identity;
```
we can rewrite it using the generic interface:
```typescript
interface GenericIdentityFn {
    <T>(arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn = identity;
```
```typescript
interface GenericIdentityFn<T> {
    <T>(arg: T): T;
}

function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: GenericIdentityFn<number> = identity<number>;
```
except from generic interface, we can create generic class.
Note that it is impossible to create a generic enum and namespace.
<br/>

## Generic Class
Generic class is similar to generic interface. Generic class use \( <\> \).
```typescript
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = (x, y) => x + y;
```
<br/>

## Generic Restriction
we may remember a previous example, and we have some times when we want to mutate a set of value of certain type and know what characteristics it has.

We want to restrict the function to deal with `any` type, rather than operate `any` type All types of the `length` attribute. As long as the incoming type has this attribute, we allow it, that is, at least this attribute is included. To do this, we need to list the constraint requirements for T:
```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length)
    return arg;
}
```
We are required to pass the value conform to restricted type, which must include required attribute:
```typescript
loggingIdentity({ length: 10, value: 3 });
```
