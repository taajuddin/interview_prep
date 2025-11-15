# 📘 TypeScript Interview Questions & Answers (100+)

This document contains **100+ TypeScript interview questions** with
clear and concise answers, helpful for beginners to advanced developers.

------------------------------------------------------------------------

## 🟢 Basic TypeScript Interview Questions (1--30)

### 1. What is TypeScript?

TypeScript is a **typed superset of JavaScript** developed by Microsoft
that adds static typing and compiles to plain JavaScript.

### 2. Why use TypeScript?

Benefits: - Early error detection - Better code readability - Improved
IDE support - Easier large-scale application development

### 3. Difference between TypeScript and JavaScript?

  TypeScript                             JavaScript
  -------------------------------------- --------------------------
  Statically typed                       Dynamically typed
  Compiles to JS                         Runs directly
  Supports interfaces, enums, generics   Not supported by default
  Best for large projects                Good for small scripts

### 4. What are basic types?

`string`, `number`, `boolean`, `null`, `undefined`, `any`, `unknown`,
`void`, `never`, `object`.

### 5. What is `any`?

Allows any value without type checking.

### 6. What is `unknown`?

A safer alternative to `any` requiring type checking.

### 7. What is `void`?

Used when a function returns nothing.

### 8. What is `never`?

Used for functions that never return.

``` ts
function throwError(): never { throw new Error("error"); }
```

### 9. What is a Union type?

``` ts
let id: number | string;
```

### 10. What is a Tuple?

``` ts
let user: [string, number] = ["Alice", 25];
```

### 11. What is an Interface?

Defines structure of objects.

### 12. What is a Type Alias?

Custom type name.

### 13. Interface vs Type

Interfaces can merge, types cannot.

### 14. What are Generics?

Reusable types.

``` ts
function identity<T>(value: T): T { return value; }
```

### 15. Type Inference

TS infers type automatically.

### 16. Enum

Named constants.

### 17. Readonly

Prevents fields from changing.

### 18. keyof operator

Returns union of keys of a type.

### 19. Optional Chaining

``` ts
user?.address?.city
```

### 20. Nullish Coalescing

``` ts
value ?? 'default'
```

### 21--30 More Topics

Union narrowing, literal types, function overloading, type guards,
intersection types, index signatures, etc.

------------------------------------------------------------------------

## 🟡 Intermediate Questions (31--70)

### 31. What are Utility Types?

`Partial`, `Pick`, `Omit`, `Record`, `Readonly`.

### 32. Partial

``` ts
Partial<User>
```

### 33. Pick

Selects specific fields.

### 34. Omit

Removes fields.

### 35. Record

Maps keys to a type.

### 36. Declaration Merging

Multiple declarations merged into one.

### 37. Conditional Types

``` ts
T extends U ? X : Y
```

### 38. Mapped Types

``` ts
type MakeOptional<T> = { [P in keyof T]?: T[P] }
```

### 39. infer keyword

Used for extracted types.

### 40. Discriminated Unions

Union with shared `type` field.

### 41--70

Covers ambient declarations, namespaces, module resolution, type
assertion, async/await typing, event typing, branded types,
exhaustiveness checks, decorators, abstract classes, mixins, etc.

------------------------------------------------------------------------

## 🔴 Advanced Questions (71--100+)

### 71. What is a Branded Type?

Prevents accidental mixing of values with same primitive type.

### 72. What are Assertion Functions?

``` ts
function assertIsNumber(value: unknown): asserts value is number {}
```

### 73. What is a Type Guard?

Checks variable type at runtime.

### 74. What is Indexed Access Type?

``` ts
type Age = User["age"];
```

### 75. What is Template Literal Type?

``` ts
type Color = `#${string}`;
```

### 76. Variadic Tuple Types

Rest elements inside tuples.

### 77--100+ Topics

JS interoperability, advanced generics, deep readonly, deep partial,
react prop types, nested object types, strongly typed API responses,
function type composition.

------------------------------------------------------------------------

## 🚀 Summary

This document prepares you for: - TypeScript Developer Roles -
React/Node/Angular/MERN/Nest.js Jobs - Full Stack Engineering Interviews

------------------------------------------------------------------------

Good luck with your interviews! 🚀
