---
title: "What Exactly Is a Type Anyway?"
date: 2021-06-28T22:20:43+01:00
draft: false
subtitle: ""
banner: https://www.plantcode.blog/me/banner.png
categories: -undefined
tags:
  - undefined
---

A type is a set of values and the things you can do with them.

- The boolean type is the set of all booleans (there are just two: true and false) and the operations you can perform on them (like ||, &&, and !).
- The number type is the set of all numbers and the operations you can perform on them (like +, -, \*, /, %, ||, &&, and ?), including the methods you can call on them like .toFixed, .toPrecision, .toString, and so on.
- The string type is the set of all strings and the operations you can perform on them (like +, ||, and &&), including the methods you can call on them like .concat and .toUpperCase.

**Type Literal**: A Type that represents a single value and nothing else.

You can either let TypeScript infer that your value is a number like:

`let a = 1234`

`var b = Infinity * 0.10`

Use `const` so TypeScript infers that your value is a specific number. Because TypeScript knows that once a primitive is assigned a const its value will never change, it infers the most narrow type is can for that variable.

`const c = 5678`

Tell TypeScript explicity that your value is a number

`let d: number = 100`

When working with long numbers, use numeric separators to make those numbers easier to read.You can use numeric separators in both type and value positions:

`let oneMillion= 1_000_000 // Equivalent to 1000000`

`let twoMillion: 2_000_000 = 2_000_000`

By default, TypeScript is pretty strict about object properties — if you say the object should have a property called `b` that’s a number, TypeScript expects b and only b. If b is missing, or if there are extra properties, TypeScript will complain.

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let a: {b: number}

a = {} // Error TS2741: Property 'b' is missing in type '{}'
// but required in type
// '{b: number}'.

a = {
b: 1,
c: 2
}

//_ Error TS2322: Type '{b: number; c: number}'
// is not assignable} to type '{b: number}'.
// Object literal may only
// specify known properties,
// and 'c' does not exist in type '{b: number}' _\.
{{< / highlight >}}

### Definite Assignment

When you declare a variable in one place and initialise it later, TypeScript will make sure that your variable is definitely assigned a value by the time you use it:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let i: number;
let j = i \* 3; //Error TS2454: Variable 'i' is used before being assigned.
{{< / highlight >}}

To tell TypeScript that something in an object is optional, or that there might be more properties than you planned for, you can use special syntax:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let a: {
b: number;
c?: string;
[key: number]: boolean;
};
{{< / highlight >}}.

1. `a` has a property `b` that's a `number`.
2. `a` _might_ have a property `c` that's a `string`. And if `c` is set, it might be `undefined`.
3. `a` might have any number of numeric properties that are `booleans`.

### Index Signatures

That final part with the `[key: T]: U` syntax is called an `index signature`, and this is the way you tell TypeScript that the given object might contain more keys.

The way to read it is, “For this object, all keys of type T must have values of type U.” Index signatures let you safely add more keys to an object, in addition to any keys that you explicitly declared.

There is one rule to keep in mind for index signatures: the index signature key’s type (T) must be assignable to either number or string.

Also note that you can use any word for the index signature key’s name — it doesn’t have to be key:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let airplaneSeatingAssignments: {
[seatNumber: string]: string;
} = {
"34D": "Boris Cherny",
"34E": "Bill Gates",
};
{{< / highlight >}}

Optional (?) isn’t the only modifier you can use when declaring object types. You can also mark fields as read-only (that is, you can **declare that a field can’t be modified after it’s assigned an initial value** — kind of like const for object properties) with the readonly modifier:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let user: {
readonly firstName: string;
} = {
firstName: "abby",
};

user.firstName; // string
user.firstName = "abbey with an e"; // Error TS2540: Cannot assign to 'firstName'
// because it is a read-only property.
{{< / highlight >}}

### Type aliases

Here we've declared a type alias 'Age' that points to a type `number`:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
type Age = number;

type Person = {
name: string;
age: age;
};
{{< / highlight >}}

### Tuples

Tuples are subtypes of array. They’re a **special way to type arrays that have fixed lengths**, where the values at each index have _specific_, _known_ types.

Unlike most other types, tuples have to be explicitly typed when you declare them. That’s because the JavaScript syntax is the same for tuples and arrays (both use square brackets), and TypeScript already has rules for inferring array types from square brackets:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let b: [string, string, number];

b = ["malcolm", "gladwell", 1963]; //this works fine and matches type

b = ["queen", "elizabeth", "ii", 1926]; // the third element is not matching type and it has one more element than the tuple length specifies
{{< / highlight >}}

Tuples support optional elements and rest elements, which are sort of like [rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) - they allow you to accept an indefinite amount of indexes into the array. Note the use of the triple full stop syntax.

For example, if we wanted to create a heterogeneous list:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let list: [number, boolean, ...string[]] = [1, false, "a", "b", "c"];
{{< / highlight >}}

### Read-only arrays and tuples

Regular arrays are mutable, sometimes you want an immutable array. For example you wouldn't want someone to accidently edit it and cause bugs. You can update a readonly array to produce a new array, leaving the original unchanged.

To create a read-only array, use an explicit type annotation; to update a read-only array, use nonmutating methods like `.concat` and `.slice` instead of mutating ones like `.push` and `.splice`:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
let as: readonly number[] = [1, 2, 3];
let bs: readonly number[] = as.concat(4);
{{< / highlight >}}

### Functions

For functions in TypeScript you will usually explicitly annotate function parameters (`a` and `b` in this example) -- TypeScript will always infer types throughout the body of your function, but in most cases it won't infer types for your parameters. The return type therefore _is_ inferred, but you can explicitly annotate it too:

{{< highlight typescript "linenos=tables,linenostart=1" >}}
function add(a: number, b: number): number {
return a + b;
}
{{< / highlight >}}

### Getters and Setters

To avoid checking all the time if a value assigned to a property is valid from user input, we use setters and getters. The getters and setters allow you to **control the access to the properties of a class**.

For each property:

- A getter method returns the value of the property’s value. A getter is also called an accessor. Keyword `get`.
- A setter method updates the property’s value. A setter is also known as a mutator. Keyword `set`.

Setters are useful when you want to validate the data before assigning it to the properties.
