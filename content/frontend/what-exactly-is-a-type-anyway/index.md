---
title: "What Exactly Is a Type Anyway?"
date: 2021-06-28T22:20:43+01:00
draft: false
subtitle: ""
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

```typescript
let a: {b: number}

a = {}  // Error TS2741: Property 'b' is missing in type '{}'// but required in type '{b: number}'.

a = {
  b: 1,
  c: 2
}

/* Error TS2322: Type '{b: number; c: number}' is not assignable} to type '{b: number}'. Object literal may only
specify known properties,
and 'c' does not exist in type '{b: number}' *\.
```

### Definite Assignment

When you declare a variable in one place and initialise it later, TypeScript will make sure that your variable is definitely assigned a value by the time you use it:

```typescript
let i: number;
let j = i * 3; //Error TS2454: Variable 'i' is used before being assigned.
```

To tell TypeScript that something in an object is optional, or that there might be more properties than you planned for, you can use special syntax:

```typescript
let a: {
  b: number;
  c?: string;
  [key: number]: boolean;
};
```

1. `a` has a property `b` that's a `number`.
2. `a` _might_ have a property `c` that's a `string`. And if `c` is set, it might be `undefined`.
3. `a` might have any number of numeric properties that are `booleans`.

### Index Signatures

That final part with the `[key: T]: U` syntax is called an `index signature`, and this is the way you tell TypeScript that the given object might contain more keys.

The way to read it is, “For this object, all keys of type T must have values of type U.” Index signatures let you safely add more keys to an object, in addition to any keys that you explicitly declared.

There is one rule to keep in mind for index signatures: the index signature key’s type (T) must be assignable to either number or string.

Also note that you can use any word for the index signature key’s name — it doesn’t have to be key:

```typescript
let airplaneSeatingAssignments: {
  [seatNumber: string]: string;
} = {
  "34D": "Boris Cherny",
  "34E": "Bill Gates",
};
```

Optional (?) isn’t the only modifier you can use when declaring object types. You can also mark fields as read-only (that is, you can **declare that a field can’t be modified after it’s assigned an initial value** — kind of like const for object properties) with the readonly modifier:

```typescript
let user: {
    readonly firstName: string
} = {
  firstName: 'abby'
}

user.firstName// string
user.firstName='abbey with an e' // Error TS2540: Cannot assign to 'firstName' because it is a read-only property.
```
### Type aliases

Here we've declared a type alias 'Age' that points to a type `number`:

```typescript
type Age = number

type Person = {
  name: string
  age: age
}
```


