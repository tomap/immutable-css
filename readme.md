<h1 align="center">
  <img width="360" src="https://rawgit.com/johnotander/immutable-css/master/media/logo.png" alt="immutable-css">
</h1>

[![Build Status](https://secure.travis-ci.org/johnotander/immutable-css.png?branch=master)](https://travis-ci.org/johnotander/immutable-css) [![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://github.com/feross/standard)

Best practices suggest avoiding overriding styles from vendor libraries to prevent unwanted side effects. Base library styles should not be altered – or as Harry Roberts describes, base styles should be treated as [Immutable CSS](http://csswizardry.com/2015/03/immutable-css/).

See the interactive [web app](http://immutablecss.com).

## Installation

```bash
npm install --save immutable-css
```

## Usage

Immutable CSS detects mutations among files by leveraging PostCSS sourcemaps. It is also best used as a PostCSS plugin in tandem with `postcss-import` and `postcss-reporter`.

```js
var fs = require('fs')
var postcss = require('postcss')
var import = require('postcss-import')
var reporter = require('postcss-reporter')
var immutableCss = require('immutable-css')

var css = fs.readFileSync('styles.css', 'utf8')

var mutations = postcss([import(), immutableCss(), reporter()])
                  .process(css, { from: 'styles.css' })
```

### Input

```css
@import 'basscss';

.button {}
.left {}
.something-else {}
```

### Output

```sh
⚠  .button was mutated 2 times
[line 93, col 1]: /css/basscss.css
[line 3, col 1]: /css/custom.css
[immutable-css]
⚠  .left was mutated 2 times
[line 291, col 1]: /css/basscss.css
[line 4, col 1]: /css/custom.css
[immutable-css]
```

### Options

* `ignoredClasses` (Array): List of classes to ignore for mutation violations. <br>Ex: `['.some-mutable-class']`
* `immutableClasses` (Array): List of classes to check against. <br>Ex: `['.button', '.foobar']`
* `immutablePrefixes` (Array): List of prefix regexes that are immutable. <br>Ex: `[/\.u\-/, /\.util\-/]`
* `callback` (Function): Callback that receives a mutations object. <br>Ex: `function (mutations) { console.log(mutations) }`

### Using the callback

Immutable CSS accepts an optional callback, which returns the mutations hash. The key is the mutated class name, the value is an array of mutating filenames.

```js
postcss([
  import(),
  immutableCss({ ignoredClasses: ['.button'] }, function(mutations) {
    console.log(mutations)
    // => { '.foobar': [] }
  })
]).process(css, { from: cssFile })
```

### Using the [immutable-css-cli](https://github.com/johnotander/immutable-css-cli)

```sh
npm i -g immutable-css-cli
```

```sh
immutable-css css/main.css
⚠  .button was mutated 2 times
[line 93, col 1]: /css/_basscss.css
[line 11, col 1]: /css/_custom.css
[immutable-css]
⚠  .left was mutated 2 times
[line 291, col 1]: /css/_basscss.css
[line 15, col 1]: /css/_custom.css
[immutable-css]
```

<https://github.com/johnotander/immutable-css-cli>

## Dependencies

* <https://github.com/postcss/postcss>
* <https://github.com/johnotander/get-css-classes>
* <https://github.com/css-modules/css-selector-tokenizer>

## Related Reading

* <http://csswizardry.com/2015/03/immutable-css/>
* <http://csswizardry.com/2012/06/the-open-closed-principle-applied-to-css/>
* <http://www.jon.gold/2015/07/functional-css/>
* <http://eng.wealthfront.com/2013/08/functional-css-fcss.html>
* <http://www.basscss.com/docs/reference/principles/#immutable-utilities>

## License

MIT

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

Crafted with <3 by [@jxnblk](https://twitter.com/jxnblk) & [@4lpine](https://twitter.com/4lpine).

***

> This package was initially generated with [yeoman](http://yeoman.io) and the [p generator](https://github.com/johnotander/generator-p.git).
