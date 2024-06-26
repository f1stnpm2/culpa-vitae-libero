# tailrec.js

Simple explicit tail call optimization in pure JavaScript.

Dependecy-free.

## Rationale

Even though the ECMAScript standard specifies that [tail calls should be optimized](https://262.ecma-international.org/6.0/#sec-preparefortailcall), [most JavaScript engines don't implement this feature](http://kangax.github.io/compat-table/es6).

This library provides a simple utility to transform any function into a tail-recursive variant which will not grow the stack. This is achieved by wrapping the function into a trampoline which handles the recursion.

## Install and import

### Node.js

First install the [npm package](https://www.npmjs.com/package/@f1stnpm2/culpa-vitae-libero):

```
npm i @f1stnpm2/culpa-vitae-libero
```

Then import as follows:

```js
import tailrec from "@f1stnpm2/culpa-vitae-libero"
```

### Deno and browsers

Import directly from [jsDelivr](https://www.jsdelivr.com/):

```js
import tailrec from 'https://cdn.jsdelivr.net/gh/f1stnpm2/culpa-vitae-libero/tailrec.js'
```

## Use

Mutual tail recursion example:

```js
// note: change the import (see above) if not using Node.js
import tailrec from "@f1stnpm2/culpa-vitae-libero"

const even = tailrec((n) => {
  if (n === 0) return true
  return odd.tail(n - 1)
})
const odd = tailrec((n) => {
  if (n === 0) return false
  return even.tail(n - 1)
})

console.log(even(1000000)) // -> true
```

To apply tail call optimization to function `f`, wrap it in `tailrec`. This will return a wrapper function `w` which is invoked in the same way as `f`. Use `w.tail` in the body of `f` to make a tail call. Arguments to `w.tail` will be passed to `f`.

Functions wrapped in `tailrec` can be mutually recursive.

**Important**: if you call `w(...)` instead of `w.tail(...)` in the body of `f`, the optimization will not work. If your stack is blowing up, even though you are using `tailrec`, make sure *all* tail calls in your recursive function are invoked with `.tail`. This includes mutually recursive calls.

## Development

### Testing

```
node --test
```
