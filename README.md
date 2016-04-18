# browserify-postcss
[![version](https://img.shields.io/npm/v/browserify-postcss.svg)](https://www.npmjs.org/package/browserify-postcss)
[![status](https://travis-ci.org/zoubin/browserify-postcss.svg)](https://travis-ci.org/zoubin/browserify-postcss)
[![coverage](https://img.shields.io/coveralls/zoubin/browserify-postcss.svg)](https://coveralls.io/github/zoubin/browserify-postcss)
[![dependencies](https://david-dm.org/zoubin/browserify-postcss.svg)](https://david-dm.org/zoubin/browserify-postcss)
[![devDependencies](https://david-dm.org/zoubin/browserify-postcss/dev-status.svg)](https://david-dm.org/zoubin/browserify-postcss#info=devDependencies)

A [browserify] transform to work with [postcss].

## Example

The build script:

```javascript
var browserify = require('browserify')
var fs = require('fs')

var to = __dirname + '/static/bundle.js'
var b = browserify(__dirname + '/src/entry.js')
b.transform(require('browserify-postcss', {
  // a list of postcss plugins
  plugin: [
    'postcss-import',
    'postcss-advanced-variables',
    ['postcss-custom-url', [
      ['inline', { maxSize: 10 }],
      ['copy', { assetOutFolder: __dirname + '/static/assets' }],
    ]],
  ],
  // basedir where to search plugins
  basedir: __dirname + '/src',
  // options for processing.
  postCssOptions: { to: to },
  // insert a style element to apply the styles
  inject: true,
})
b.bundle().pipe(
  fs.createWriteStream(to)
)

```

entry.js:

```js
require('./entry.css')

console.log('styles from entry.css are applied')

```

## Options

### plugin
Specify a list of [postcss] plugins to apply.

Type: `String`, `Array`
Default: `null`

### basedir
Specify where to look for plugins.

### postCssOptions
Specify the options for the [postcss] processor.

The `from` and `to` fields will be set to the css file path by default.

### inject
Specify how to use the styles:

If `true`, styles are applied immediately.
If `false`, `require('style.css')` will return the string representation of the styles.

### extensions
A list of file extensions to identify styles.

Type: `Array`

Default: `['.css', '.scss', '.sass']`

## Watch
Imported files will **NOT** be watched when used with [watchify].

## Related

* [reduce-css]: bundle css files without `require`ing them in js.


[browserify]: https://github.com/substack/node-browserify
[watchify]: https://github.com/substack/watchify
[postcss]: https://github.com/postcss/postcss
[reduce-css]: https://github.com/reducejs/reduce-css
