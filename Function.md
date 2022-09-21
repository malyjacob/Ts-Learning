# Function
## Function Type
Let's add type for a function:
```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };
```
We can add return type for function after adding parameters types. Typescript can infer the return type of a function automatically according to the statement. So we often omit it.

### Write Complete Function Type
Let's write down the complete type of a function:
```typescript
let myAdd: (x: number, y: number) => number = function (x: number, y: number): number { return x + y; };
```
Function type includes two part: parameter type and return type. when write down the complete type of a function, the two part both needed.

For Convenience, we can rewrite the function above like this:
```typescript
let myAdd: (x: number, y: number) => number = function (x, y){ return x + y; };
```
<br/>

## Optional Parameters and Default Parameters
In JavaScript, every parameter is optional, we can pass a value to it or not. When not passing value, the parameter' s value is `undefined`.

In Typescript, we can use `?` after the parameter name to implement the function of optional parameter. For example, we want to make `lastName` is optional:
```typescript
function buildName(firstName: string, lastName?: string) {
    if(lastName) return firstName + ' ' + lastName;
    else return firstName;
}
let result1 = buildName("Bob");  // works correctly now
let result2 = buildName("Bob", "Adams", "Sr.");  // error, too many parameters
let result3 = buildName("Bob", "Adams");  // ah, just right
```
Optional parameters must follow required parameters.

Parameters with default initialization after all required parameters are optional, and like optional parameters, can be omitted when calling the function.
That is, the optional parameter shares the parameter type with the default parameter at the end.
```typescript
function buildName(firstName: string, lastName: string) {
    / ... 
}
```
ANd
```typescript
function buildName(firstName: string, lastName = "Smith") {
    // ...
} 
```
The two share the same type: `(firstName: string, lastName: string) => string`.
<br/>

## Rest Parameters
Both required parameters and default parameters have a common: they both represent a parameter.

Sometimes you may want to mutate more than one parameter at the same time, or you do not know how many parameters will be passed.

In Typescript, you can collect all the parameters in a variable:
```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(' ');
}
```
The remaining parameters are treated as an unlimited number of optional parameters.
There can be none, and there can be any.
<br/>

## `this`
### `this` parameter
To avoid the problem we often encounter that we may not be able to know what exactly `this` is, we can provide the function an explicit `this` parameter. `this` parameter is a virtual parameter which appears the front of parameter list.
```typescript
function f(this: void) {
    // ...
}
```
```typescript
interface Card {
    suit: string;
    card: number;
}
interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}
let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    // NOTE: The function now explicitly specifies that its callee must be of type Deck
    createCardPicker: function(this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
}

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);
```
Now Typescript compiler know `createCardPicker` expect the call on a `Deck` object. That is, `this` is `Deck` type, but not `any`.

### `this` parameter in callback function
Note that the `this` in callback must be consistent with the outer function's `this`.
<br/>

## Reload
JavaScript itself is a dynamic language, It is very common in JavaScript for functions to return different types of data depending on the parameters passed in.

In Typescript, the same function can provide multiple function type definitions for function reloading.

```typescript
let suits = ["hearts", "spades", "clubs", "diamonds"];

function pickCard(x: {suit: string; card: number; }[]): number;
function pickCard(x: number): {suit: string; card: number; };
function pickCard(x): any {
    // Check to see if we're working with an object/array
    // if so, they gave us the deck and we'll pick the card
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    // Otherwise just let them pick the card
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x / 13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

let myDeck = [{ suit: "diamonds", card: 2 }, { suit: "spades", card: 10 }, { suit: "hearts", card: 4 }];
let pickedCard1 = myDeck[pickCard(myDeck)];
alert("card: " + pickedCard1.card + " of " + pickedCard1.suit);

let pickedCard2 = pickCard(15);
alert("card: " + pickedCard2.card + " of " + pickedCard2.suit);
```

To let compiler be able to select true type checking, it behave like the dispose process in JavaScript. It will search reload list, try using the first reload definition. If match, just use it.

Therefore, we must put the most exact definition at the front.