[![Build Status](https://travis-ci.org/kaelzhang/assets-html-webpack-plugin.svg?branch=master)](https://travis-ci.org/kaelzhang/assets-html-webpack-plugin)
<!-- optional appveyor tst
[![Windows Build Status](https://ci.appveyor.com/api/projects/status/github/kaelzhang/assets-html-webpack-plugin?branch=master&svg=true)](https://ci.appveyor.com/project/kaelzhang/assets-html-webpack-plugin)
-->
<!-- optional npm version
[![NPM version](https://badge.fury.io/js/assets-html-webpack-plugin.svg)](http://badge.fury.io/js/assets-html-webpack-plugin)
-->
<!-- optional npm downloads
[![npm module downloads per month](http://img.shields.io/npm/dm/assets-html-webpack-plugin.svg)](https://www.npmjs.org/package/assets-html-webpack-plugin)
-->
<!-- optional dependency status
[![Dependency Status](https://david-dm.org/kaelzhang/assets-html-webpack-plugin.svg)](https://david-dm.org/kaelzhang/assets-html-webpack-plugin)
-->

# assets-html-webpack-plugin

Handle JavaScript or CSS assets to the HTML, woking with [html-webpack-plugin](https://www.npmjs.com/package/html-webpack-plugin)

- Supports to add vendors to html pages.
- Fallback to global output configuration of webpack.

## Install

```sh
$ npm install assets-html-webpack-plugin --save-dev
```

## Usage

```js
// html-webpack-plugin@^2.10.0 is required
const HtmlPlugin = require('html-webpack-plugin')
const AssetsPlugin = require('assets-html-webpack-plugin')
const webpackConfig = {
  output: {
    path: '/path/to',
    ...
  },
  plugins: [
    new HtmlPlugin(...),
    new AssetsPlugin({
      // Suppose the filepath is '/src/foo.js'
      assets: [require.resolve('./path/to/foo')],
      chunks: ['vendor'],
      output: {
        filename: 's/[name].[chunkhash].js',
        publicPath: '//mycdn.com/m'
      }
    })
  ]
}
```

Then, `/src/foo.js` will be copied to

```
/path/to/s/
          |-- foo.26313ef12faa88b00420.js
```

And the following script tags will be injected into the html:

```html
<script
  type=text/javascript
  src=//mycdn.com/m/s/vendor.c0624bf9273d8c3b40a8.js></script>
<script
  type=text/javascript
  src=//mycdn.com/m/s/foo.26313ef12faa88b00420.js></script>
```

Notice that `assets` scripts will come first, then `chunks`.

## new AssetsPlugin(options [, options, ...])

**options** `Object`

- **assets** `Array.<Asset|Path>`
- **chunks** `Array.<String>`

If both `assets` and `chunks` are empty or not defined, an error will throw.   

- **output.filename** `String` default to `webpack.config.output.filename`. Which will not affect `chunks`.
- **output.publicPath** `String` default to `webpack.config.output.publicPath`
- **append** `Boolean=false` whether the assets will be append to the end of the existing assets
- **typeOfAsset** `String.<js|css>=` Optional. Specify the type of the asset. By default, `AssetPlugin` will detect the extname of the filepath.


## License

MIT
