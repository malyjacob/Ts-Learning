# Class

## Class

Let's look at an example using class:

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet(0 {
        return "Hello, " + this.greeting;
    })
}

let greeter = new Greeter("world");
```

<br/>

## Inherit

One of most basic pattern in class based programming is to allow the use of inheritance to extend existing classes.

Take the following example:

```typescript
class Animal {
  move(distanceInMeters: number = 0) {
    console.log("Animal mover ${distanceInMeters}");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Woof! Woof!");
  }
}

const dog = new Dog();
dog.bark();
dog.move(10);
```

Class Inherit properties and methods from the base class.

Here, `Dog` is a derived class, it derive from base class `Animal` through keyword `extends`.

Derived class is usually called as subclass, base class as superclass.

Let's look at a more complex example:

```typescript
class Animal {
  name: string;
  constructor(theName: string) {
    this.name = theName;
  }
  move(distanceInMeters: number = 0) {
    console.log(`${this.name} moved ${distanceInMeters}m.`);
  }
}

class Snake extends Animal {
  constructor(name: string) {
    super(name);
  }
  move(distanceInMeters = 5) {
    console.log("Slithering...");
    super.move(distanceInMeters);
  }
}

class Horse extends Animal {
  constructor(name: string) {
    super(name);
  }
  move(distanceInMeters = 45) {
    console.log("Galloping...");
    super.move(distanceInMeters);
  }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

Different from last example, derived class includes a constructor function, it must call `super` function, and it will execute the constructor function of base class.

And before constructor function access `this`, we must call `super`.

This example has shown how to override the methods of superclass. Class `Sake` and class `House` build the method `move`, and they both override method `move` derived from class `Animal`, which make method `move` has various function from different class.

Note that event if `tom` is declared as tye `Animal`, it will call the methods overridden in `House` when calling `tom.move(34)`, because its value is still `House`.
<br/>

## Public, Private and Protected Modifier

### `public` by default

In TypeScript, members are public by default.

And you can explicitly specify a member as `public`:

```typescript
class Animal {
  public name: string;
  public constructor(theName: string) {
    this.name = theName;
  }
  public move(distanceInMeters: number) {
    console.log(`${this.name} move ${distanceInMeters}m.`);
    return distanceInMeters;
  }
}
```

### understand `private`

when members are declared as `private`, they are not able to be accessed from the outer of the class that declares it.

for example:

```typescript
class Animal {
  private name: string;
  constructor(theNAme: string) {
    this.name = theNAme;
  }
  getName() {
    return this.name;
  }
}

let cat = new Animal("Cat");
cat.name; // error: `ame` is private
cat.getName(); // => "Cat"
```

### understand `protected`

The protected modifier behaves similarly to the private modifier, but with one difference, protected members are still accessible in derived classes.
E.g:

```typescript
class Person {
  protected name: string;
  constructor(theName: string) {
    this.name = theName;
  }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my ame is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
console.log(howard.name)  // error
```
Note that we cannot use `name` outside of class `Person`, but we can access it by the instance method of class `Employee`, because `Employee` is derived from `Person`.

constructor function can als be specified as `protected`, which means this class is not able to be instantiated outside of the class which contains it. However, it can be Inherited. E.g:
```typescript
class Person {
  protected name: string;
  protected constructor(theName: string) {
    this.name = theName;
  }
}

class Employee extends Person {
    private department: string;

    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }

    public getElevatorPitch() {
        return `Hello, my ame is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
let john = new Person("John"); // error: the constructor of 'Person' is protected. 
```
<br/>

## readonly modifier
You can use `readonly` keyword to make the properties as read-only.

Read-only property must be initialized  when being declared or in the constructor function.
```typescript
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor(theName: string) {
        this.name = theName;
    }
}

let dad = new Octopus("Man with the 8 string legs");
dad.name = "Man with the 3-piece suit"; // error: name is read-only.
```
### parameter property
In the example above, we must define a read-only member in the class `Octopus` and a constructor function with a parameter called `theName`,and assign the value of `theNAme` to the property `name`.

However, parameter property can make u define and initialize a member at only one place conveniently.

The following example is the modified version of previous class `Octopus` using parameter property:
```typescript
class Octopus {
    readonly numberOfLegs: number = 8;
    constructor(readonly name: string) {
    }
}
```
Parameter properties are declared by prepending an access qualifier to the constructor parameter.
Defining a parameter property with private declares and initializes a private member; the same is true for public and protected.
<br/>

## Getter and setter
Type Script supports intercepting access to object members through getters/setters.

Let's see how to rewrite a simple class to use `get` and `set`.Firstly, let's begin with an example not using getters/setters.
```typescript
class Employee {
    fullName: string;
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```
The following is the modified one with getter and setter:
```typescript
let passcode = "secret passcode";

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (passcode && passcode == "secret passcode") {
            this._fullName = newName;
        }
        else {
            console.log("Error: Unauthorized update of employee!");
        }
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    alert(employee.fullName);
}
```
Note that accessors with only `get` but not `set` are automatically inferred to be `readonly`.
<br/>

## Static Property
We can also create static members of class, which is in class it-self instead of the instance of the class.
```typescript
class Grid {
    static origin = {x: 0, y: 0};
    calculateDistanceFromOrigin(point: {x: number; y: number;}) {
        let xDist = (point.x - Grid.origin.x);
        let yDist = (point.y - Grid.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    constructor (public scale: number) { }
}

let grid1 = new Grid(1.0);  // 1x scale
let grid2 = new Grid(5.0);  // 5x scale

console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(grid2.calculateDistanceFromOrigin({x: 10, y: 10}));
```
<br/>

## Abstract Class
Abstract classes are used as base classes for other derived classes. Usually, they are not directly instantiated.

Unlike interface, they can contains the details of members' implementation.

`abstract` keyword is used to define abstract class and the abstract methods defined in this class. E.g:
```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earch...');
    }
}
```
An abstract method in an abstract class does not contain a concrete implementation and must be implemented in a derived class.
The syntax of abstract methods is similar to interface methods.
Both define the method signature but not the method body.
However, abstract methods must contain the abstract keyword and can contain access modifiers.
```typescript
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log('Department name: ' + this.name);
    }

    abstract printMeeting(): void; // 必须在派生类中实现
}

class AccountingDepartment extends Department {

    constructor() {
        super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
    }

    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }

    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let department: Department; // 允许创建一个对抽象类型的引用
department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
department.printName();
department.printMeeting();
department.generateReports(); // 错误: 方法在声明的抽象类中不存在
```
         
