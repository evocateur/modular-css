modular-css-svelte  [![NPM Version](https://img.shields.io/npm/v/modular-css-svelte.svg)](https://www.npmjs.com/package/modular-css-svelte) [![NPM License](https://img.shields.io/npm/l/modular-css-svelte.svg)](https://www.npmjs.com/package/modular-css-svelte) [![NPM Downloads](https://img.shields.io/npm/dm/modular-css-svelte.svg)](https://www.npmjs.com/package/modular-css-svelte)
===========

<p align="center">
    <a href="https://gitter.im/modular-css/modular-css"><img src="https://img.shields.io/gitter/room/modular-css/modular-css.svg" alt="Gitter" /></a>
</p>

Svelte preprocessor support for [`modular-css`](https://github.com/tivac/modular-css). Process inline `<style>`s inside your Svelte components using the full power of `modular-css` while also providing compile-time optimizations for smaller bundles and even faster runtime performance!

## Example

Turns this

```html
<div class="{{css.main}}">
    <h1 class="{{css.title}}">Title</h1>
</div>

<style>
    .main {
        ...
    }
    
    .title {
        ...
    }
</style>
```

into what is effectively this

```html
<div class="abc123_main">
    <h1 class="abc123_title">Title</h1>
</div>
```

while allowing you to use all of the usual `modular-css` goodies.

## Install

`$ npm i modular-css-svelte`

## Usage

### `svelte.preprocess()`

```js
const filename = "./Component.html";

const { processor, preprocess } = require("modular-css-svelte")({
    css : "./dist/bundle.css"
});

const processed = await svelte.preprocess(
    fs.readFileSync(filename, "utf8"),
    Object.assign({}, preprocess, { filename })
);

const result = processor.output();

fs.writeFileSync("./dist/bundle.css", result.css);
```

### `rollup-plugin-svelte`

#### API

```js
const rollup = require("rollup").rollup;

const { preprocess, plugin } = require("modular-css-svelte/rollup")({
    css : "./dist/bundle.css"
});

const bundle = await rollup({
    input   : "./Component.html",
    plugins : [
        require("rollup-plugin-svelte")({
            preprocess
        }),
        plugin
    ]
});

// bundle.write will also write out the CSS to the path specified in the `css` arg
bundle.write({
    format : "es",
    file   : "./dist/bundle.js"
});
```

#### `rollup.config.js`

```js
const { preprocess, plugin } = require("modular-css-svelte/rollup")({
    css : "./dist/bundle.css"
});

module.exports = {
    input   : "./Component.html",
    output  : {
        format : "es",
        file   : "./dist/bundle.js"
    },
    plugins : [
        require("rollup-plugin-svelte")({
            preprocess
        }),
        plugin
    ]
};
```

## Options

All options are passed to the underlying `Processor` instance, see [Options](https://github.com/tivac/modular-css/blob/master/docs/api.md#options).

