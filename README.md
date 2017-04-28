
# Rollup-Multiple

A very simple CLI wrapper around Rollup that behaves exactly like the Rollup CLI (in fact, the code is identical, for all intents and purposes, a fork).

## Installation

```
npm install --save-dev rollup-multiple
```

## Usage

All options are identical to Rollup CLI. Rollup-Multiple is backwards compatible, and supports the usual single options object. Additionally, Rollup-Multiple allows `rollup.config.js` to export an array of options objects to process in parallel. So you can now do this:

```
export default [{
    entry: './common.js',
    dest: './dist/common.js',
    ...etc,
}, {
    entry: './app.js',
    dest: './dist/app.js',
    ...etc,
}, {
    entry: './landing.js',
    dest: './dist/landing.js',
    ...etc,
}]
```

It's handy to define the shared options before the individual options so they can be referenced like this (some day, this will be cleaner when Node gets the `...spread` operator):

```
const shared = {
    plugins: [],
    globals: [],
}

export default [
    Object.assign({}, shared, {
        entry: './common.js',
        dest: './dist/common.js',
    }),
    Object.assign({}, shared, {
        entry: './app.js',
        dest: './dist/app.js',
    }),
    Object.assign({}, shared, {
        entry: './landing.js',
        dest: './dist/landing.js',
    }),
]
```

## Notes

There's a PR open to merge this functionality into Rollup CLI, see [#1389](https://github.com/rollup/rollup/pull/1389). Meanwhile, I'll try to maintain this, tracking new Rollup releases.

There's no optimizing done to run two builds in parallel, they are simply rolled at the same time. The total difference between the two codebases is about 10 lines of code to handle the case where the config is an array.
