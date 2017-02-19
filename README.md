# redux-saga-effects
Small package to help import `effects` from [redux-saga](https://github.com/redux-saga/redux-saga/) package.

## Installation
```
npm i redux-saga-effects
```

## Motivation
You are using:
* Webpack 2 with Tree-Shaking or Rollup (your config is working with `jsnext:main` field)
* `redux-saga` (which is great btw!)

And you have code like this:
```javascript
import {
  call,
} from 'redux-saga/effects';

function* someSaga() {
  yield call(anotherFunction, 'param1');
}
```

All is OK, except for... this is not working well with `jsnext:main`.

Details is [here](https://github.com/redux-saga/redux-saga/blob/5fa5d628a80bbb4bfe488288b3ed19b5396f4d14/docs/Troubleshooting.md#bundling-my-saga-with-rollupjs-fails).

Much more details can be found in related issue https://github.com/redux-saga/redux-saga/issues/689.

Duplication of `redux-saga` adds ~30Kb to your bundle size (unminified, not compressed).

If you are not using Webpack 2 or Rollup or just don't care about your bundle size - all is fine, this package just not for you! 😉

But what to do if you need this to be working well and don't want to type more?.. 🤔


## Possible solutions for proper work with `jsnext:main`:
### 1. Destructure effects later
```javascript
import {
  effects, // <---- changed!
} from 'redux-saga';

const { // <---- added!
  call, // <---- added!
} = effects; // <---- added!

function* someSaga() {
  yield call(anotherFunction, 'param1');
}
```

In this case destructuring like that should take place in each file - it's OK, but need something better

### 2. Just use effects without destructuring

```javascript
import {
  effects, // <--- changed
} from 'redux-saga/effects';

function* someSaga() {
  yield effects.call(anotherFunction, 'param1'); // <--- changed
}
```

If you OK with this style - it's good solution. But you need to type more. And read more, actually.
But here is another option!

### 3. Use this package

```javascript
import {
  call,
} from 'redux-saga-effects'; // <--- changed

function* someSaga() {
  yield call(anotherFunction, 'param1');
}
```

Look, just one symbol changed! 🔮

## Benefits you get using this package:
* convinient API - all docs is coming from `redux-saga` itself, no additional wrappers, just re-export
* easy to migrate with existing codebase
* reduce bundle size for free - package is just 20 lines of code. Compare this with ~30Kb from duplication of `redux-saga`... 🤓

**Downsides**: install "one more package"
