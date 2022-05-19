# Learning TypeScript


# Learning TypeScript

### type & interface

#### type

**A type alias is exactly that - a *name* for any *type*.**

We can use a type alias to give a name to any type at all, not just an object type.

```typescript
type ID = number | string;
```

But note that, aliases are only aliases. We can't use type aliases to create different/distinct versions of the same type.

#### Interface

**An interface declaration is another way to name *an object type.***

Being concerned only with the structure and capabilities of types is why we call TypeScript a *structurally typed* type system.

#### Differences

Type aliases and interfaces are very similar. In most cases we can choose between them freely.

The key distinction is that a type can't be re-opened to add new properties while an interface is always extendable.

Other differences:

- Type aliases may not participate in **declaration merging**, but interfaces can.

- Interfaces can only be used to **declare the shapes of objects, not rename primitives**.

- Interface names will always appear in their original form in error messages.

- We can declare a type alias using `typeof`

  ```typescript
  type A = typeof B;
  ```

For the most part, we can choose based on personal preference, and TypeScript will tell you if it needs something to be the other kind of declaration. If you would like a heuristic, use `interface` until you need to use features from `type`.

#### Summary

In a word, use `interface` to describe the data structure and use `type` to describe the type relationship.




















