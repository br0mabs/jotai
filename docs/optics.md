This doc describes `jotai/optics` bundle. 

## Install

You have to install `optics-ts` to access this bundle and its functions.

```
npm install optics-ts
# or 
yarn add optics-ts
```

## focusAtom

`focusAtom` creates a new atom, based on the focus that you pass to it. This creates a derived atom that will focus on the specified part of the atom, 
and when the derived atom is updated, the derivee is notified of the update, and the equivalent update is done on the derivee.

See this: 

```typescript
const baseAtom = atom({a: 5}) // PrimitiveAtom<{a: number}>
const derivedAtom = focusAtom(baseAtom, optic => optic.prop('a')) // PrimitiveAtom<number>
```

So basically, we started with a `PrimitiveAtom<{a: number}>`, which has a getter and a setter, and then used `focusAtom` to zoom in on the `a`-property of 
the `baseAtom`, and got a `PrimitiveAtom<number>`. What is noteworthy here is that this `derivedAtom` is not only a getter, it is also a setter. If `derivedAtom` is updated, then
then equivalent update is done on the `baseAtom`. 

The example below is simple, but it's a starting point. `focusAtom` supports many kinds of optics, including `Lens`, `Prism`, `Isomorphism`.

To see more advanced optics, please see the example at: https://github.com/akheron/optics-ts


### Example

```js
import { atom } from 'jotai'
import { focusAtom } from 'jotai/optics'

const objectAtom = atom({a: 5, b: 10})
const aAtom = focusAtom(objectAtom, optic => optic.prop('a'))
const bAtom = focusAtom(objectAtom, optic => optic.prop('b'))

const Controls = () => {
  const [a, setA] = useAtom(aAtom)
  const [b, setB] = useAtom(aAtom)
  return (
    <div>
      <span>
        Value of a: {a}
      </span>
      <span>
        Value of b: {b}
      </span>
      <button onClick={() => setA(oldA => oldA + 1)}>Increment a</button>
      <button onClick={() => setA(oldB => oldB + 1)}>Increment b</button>
    </div> 
  )
}
```