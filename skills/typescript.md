## Naming

Singular names for unions, plural names for collections:

```ts
type Animal = 'dog' | 'cat' | 'bunny'
type Animals = Array<Animal>
```

### Generics

`camelCase` names for generics:

```ts
// prettier-ignore
type Emojify<animal extends Animal> =
  animal extends 'dog' ? '🐶' :
  animal extends 'cat' ? '🐱' :
  animal extends 'bunny' ? '🐰' :
  never

function emojify<animal extends Animal>(animal: animal): Emojify<animal> {
  if (animal === 'dog') return '🐶'
  if (animal === 'cat') return '🐱'
  if (animal === 'bunny') return '🐰'
  throw new Error(`Unrecognized animal: '${animal}'`)
}
```

**Why:**

I often used to start by naming the type `Animal`,
but then I would get to the generic and also want to name it `Animal`.

So I'd go back and rename the type to `AnyAnimal`.
But using "any" in the name was confusing; `UnknownAnimal` would be better but too unwieldy.

Instead, think of a generic as an "instance" of a broader type.
Then, we can reuse the "camelCase for instances, PascalCase for classes" convention.

## Union distribution

TypeScript has [distributive conditional types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html#distributive-conditional-types).

So if a generic is the only thing on the left-hand side of `extends` in a conditional type,
TS will apply the conditions for each member of a union separately:

```ts
type StringOrNumber = string | number

// No `T extends` -> not distributive
type ToArray1<T> = Array<T>
type Result1 = ToArray1<StringOrNumber>
//   ^? Array<string | number>

// `T extends` -> distributive
type ToArray2<T> = T extends any ? Array<T> : never
type Result2 = ToArray2<StringOrNumber>
//   ^? Array<string> | Array<number>

// Wrap both sides of `extends` in the same type to avoid distribution
// Most commonly, this is done with an array type because it is succinct to add `[` and `]`
type ToArray3<T> = [T] extends [any] ? Array<T> : never
type Result3 = ToArray3<StringOrNumber>
//   ^? Array<string | number>
```
