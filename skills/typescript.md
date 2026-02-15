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
