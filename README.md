# serviceworker-webpack-plugin

> Simplifies creation of a service worker to serve your webpack bundles

[![npm version](https://img.shields.io/npm/v/serviceworker-webpack-plugin.svg?style=flat-square)](https://www.npmjs.com/package/serviceworker-webpack-plugin)
[![npm downloads](https://img.shields.io/npm/dm/serviceworker-webpack-plugin.svg?style=flat-square)](https://www.npmjs.com/package/serviceworker-webpack-plugin)
[![Build Status](https://travis-ci.org/oliviertassinari/serviceworker-webpack-plugin.svg?branch=master)](https://travis-ci.org/oliviertassinari/serviceworker-webpack-plugin)

[![Dependencies](https://img.shields.io/david/oliviertassinari/serviceworker-webpack-plugin.svg?style=flat-square)](https://david-dm.org/oliviertassinari/serviceworker-webpack-plugin)
[![DevDependencies](https://img.shields.io/david/dev/oliviertassinari/serviceworker-webpack-plugin.svg?style=flat-square)](https://david-dm.org/oliviertassinari/serviceworker-webpack-plugin#info=devDependencies&view=list)

## Installation

```sh
npm install serviceworker-webpack-plugin
```

## The problem solved

When building a service worker, you probably want to [cache all](https://github.com/oliviertassinari/serviceworker-webpack-plugin/blob/master/example/src/sw.js#L38)
your assets during the `install` phase.
But in order to do so, you need their **names**.
That's not simple when you are using *Webpack*:
- The assets names are *non-deterministic* when taking advantage of the long term caching.
- The assets list can even evolve over time as you add splitting points or more resources.
- You want to keep the build process as simple as possible.

## Setup

### 1. Add the plugin to your webpack config

```js
import ServiceWorkerWepbackPlugin from 'serviceworker-webpack-plugin';

...

  plugins: [
    new ServiceWorkerWepbackPlugin({
      entry: path.join(__dirname, 'src/sw.js'),
    }),
  ],

```

### 2. Register the service worker in your main JS thread

```js
import runtime from 'serviceworker-webpack-plugin/runtime';

if ('serviceWorker' in navigator) {
  const registration = runtime.register();
}
```

### 3. Write your own `sw.js`

You can now use the global `serviceWorkerOption` variable in your `sw.js`.
E.g. In our example this object look like:
```js
{
  assets: [
    './main.256334452761ef349e91.js',
  ],
}
```

## Simple example

You can have a look at the [`/example`](https://github.com/oliviertassinari/serviceworker-webpack-plugin/tree/master/example)
folder if you need a full working example.

## API

### `ServiceWorkerWepbackPlugin(options)`

- `options`
 - `entry`, **required**, *string*:
Path to the actual service worker implementation.
 - `filename`, *string*, default `'sw.js'`:
Relative (from the webpack's config `output.path`) output path for emitted script.
 - `excludes`, *array*, default `['**/.*', '**/*.map']`:
Excludes matches assets from being added to the `serviceWorkerOption.assets` variable.
 - `relativePaths`, *boolean*, default `true`:
When set to `true`, all cache asset paths are generated relatively to the ServiceWorker
file and `publichPath` option is ignored.
 - `publicPath`, *string*, default `''`:
specifies the public URL address of the output files when referenced in a browser.

### `runtime(options)`

- `options`: That's forwarded to the `options` argument of the
[`ServiceWorkerContainer.register()`](https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register) function.

## Credit

- The [offline-plugin](https://github.com/NekR/offline-plugin) package
was a great source of inspiration.
- The [html-webpack-plugin](https://github.com/ampedandwired/html-webpack-plugin)
package was also really helpful.

## License

MIT
