# Advanced Type

## Intersection Types

Intersection type is to combine multiple types into one type.

We often use Intersection type when we encounter the occasion where it is not suitable to use classical type.

Take an example:

```typescript
function extend<T, U>(first: T, second: U): T & U {
  let result = <T & U>{};
  for (let id in first) {
    (<any>result)[id] = (<any>first)[id];
  }
  for (let id in second) {
    if (!result.hasOwnProperty(id)) {
      (<any>result)[id] = (<any>second)[id];
    }
  }
  return result;
}

class Person {
  constructor(public name: string) {}
}
interface Loggable {
  log(): void;
}
class ConsoleLogger implements Loggable {
  log() {
    // ...
  }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```

<br/>

## Union Type

Union types are closely related to intersection types, but are used quite differently.

```typescript
/**
 * Takes a string and adds "padding" to the left.
 * If 'padding' is a string, then 'padding' is appended to the left side.
 * If 'padding' is a number, then that number of spaces is added to the left side.
 */
function padLeft(value: string, padding: string | number) {
  // ...
}

let indentedString = padLeft("Hello world", true); // errors during compilation
```

Union types indicate that a value can be one of several types.
We separate each type with a vertical bar ( `|` ), so `number | string | boolean` means a value can be a `number`, `string`, or `boolean`.

If a value is union type, we can only access the shared members in all types in union type.

```typescript
interface Bird {
  fly();
  layEggs();
}

interface Fish {
  swim();
  layEggs();
}

function getSmallPet(): Fish | Bird {
  // ...
}

let pet = getSmallPet();
pet.layEggs(); // okay
pet.swim(); // errors
```

To make this code work, we will use type inference:

```typescript
let pet = getSmallPet();

if ((<Fish>pet).swim) {
  (<Fish>pet).swim();
} else {
    (<Bird>pet).fly();
}
```
typescript has two kinds of special type, that is , `null` and `undefined`. By default, type checker assume that `null` and `undefined` can be assigned to any types, which means that we can not stop assigning them to any other types.

In typescript, we can use command-line `--strickNullChecks` to resolve this problem. When you declare a variable, it will not include `null` and `undefined`.

You can use union type to explicitly declare it including `null` and `undefined`.
```typescript
let s = "foo";
s = null; // error, 'null' cannot be assigned to 'string'
let sn: string | null = "bar"; 
sn = null; // okay;

sn = undefined; // error.
```
### Optional property and Optional parameters
when usig `--strickNullChecks`, optional parameters will be added `| undefined` automatically:
```typescript
function f(x: number, y?: number) {
  return x + ( y || 0);
}

f(1,2)
f(1)
f(1,undefined)
f(1, null) // error, 'null' is not assignable tp 'number | undefined'
```
It is the same when encountering the optional property.
```typescript
class C {
  a: number;
  b?: number;
}

let c = new C();
c.a = 12;
c.a = undefined; // error
c.b = 13;
c.b = undefined; // ok
c.b = null; // error
```
<br/>

## Alias of Type
Type alias will create a new name for a type. Alias type sometimes is like interface, but it can be used to origin type, union type, tuple and any other types that you may need to manually write down a type.
```typescript
type Nme = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;

function getName(n: NameOrResolver): Name {
  if(typeof n === 'string') return n;
  else return n();
}
```
Aliasing doesn't create a new type - it creates a new name to refer to that type.

Like interface, type alias also can be generic -- we can add type parameters and pass it onto the alias declaration' s right side.
```typescript
type Container<T> = { value: T };
```
we can also use type alias to refer itself in property:
```typescript
type Tree<T> = {
  value: T;
  left: Tree<T>;
  right: Tree<T>;
}
```

### interface vs type alias
Though type alias is similar to interface, there are some differences between type alias and interface.

First, interface creates a new name, which can be used anywhere, while type alias doesn't create a new name.

In the following example, when we put mouse on `interfaced`, it will appear that it returns `Interface`, but when we put mouse on `aliased`, it will appear a object literal type.
```typescript
type Alias = { num: number }
interface Interface {
  num: number;
}

declare function aliased(arg: Alias): Alias;
declare function interfaced(arg: Interface): Interface;
```
Second, type alias can not be `extends` and `implements` (it can not `extends` and `implements` other types in return).

You should try to use interfaces instead of type aliases.