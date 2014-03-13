# node-slick-temp

Create and remove temporary directories. Useful for build tools, like Broccoli
plugins. Smart about naming, and placing them in `./tmp` if possible, so you
don't have to worry about this.

Note: This project is a rewrite using exclusively asynchronous file functions
of Jo Liss's quick-temp.

## Installation

```bash
npm install --save slick-temp
```

## Usage

```js
var slickTemp = require('slick-temp')
```

### Creating a temporary directory

To make a temporary and assign its path to `this.tmpDestDir`, call either one
of these:

```js
slickTemp.makeOrRemake(this, 'tmpDestDir').then(...)
// or
slickTemp.makeOrReuse(this, 'tmpDestDir').then(...)
```

If `this.tmpDestDir` already contains a path, `makeOrRemake` will remove it
first and then create a new directory (with a different path), whereas
`makeOrReuse` will be a no-op.

Both functions return a promise that resolves to the path of the temporary directory.

### Removing a temporary directory

To remove a previously-created temporary directory and all its contents, call

```js
slickTemp.remove(this, 'tmpDestDir').then(...)
```

This will also assign `this.tmpDestDir = null`. If `this.tmpDestDir` is
already null or undefined, it will be a no-op.

## Why asynchronous code
- Blocking the main thread is considered bad practice
- Although SSDs are pretty fast, spinning hard drives, exteral devices and
network devices have considerably more lag
- The speed difference between memory speed and CPU speed has grown steadily
in the past because memory capacity and price are considered increasingly
more important. That gap will likely continue to grow.
- Asynchronous code allows for more flexibily
  - Offloading of the work into worker threads is possible
  - Failed operations can be retried at lesser cost (main thread never blocked)

## Development
- `npm test` runs tests once
- `npm run-script autotest` runs tests on every file change

## A word of thanks
Thanks goes to Jo Liss for the idea to this library.
