# immutable-css [![Build Status](https://secure.travis-ci.org/johnotander/immutable-css.png?branch=master)](https://travis-ci.org/johnotander/immutable-css)

Best practices suggest avoiding overriding styles from vendor libraries to prevent unwanted side effects. Base library styles should not be altered – or as Harry Roberts describes, base styles should be treated as [Immutable CSS](http://csswizardry.com/2015/03/immutable-css/).

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

### Options

* `ignoredClasses` (Array): List of classes to ignore for mutation violations. Ex: `['.some-mutable-class']`
* `immutableClasses` (Array): List of classes to check against. Ex: `['.button', '.foobar']`
* `immutablePrefixes` (Array): List of prefix regexes that are immutable. Ex: `[/\.u\-/, /\.util\-/]`

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

### Using the CLI

```
npm i -g immutable-css
```

```
immutable-css vendor.css app.css app2.csscss-mutations.css
⚠  .button was mutated 2 times
[line 93, col 1]: /css/immutable-css/test/fixtures/basscss.css
[line 11, col 1]: /css/immutable-css/basscss-mutations.css
[immutable-css]
⚠  .left was mutated 2 times
[line 291, col 1]: /css/immutable-css/test/fixtures/basscss.css
[line 15, col 1]: /css/immutable-css/basscss-mutations.css
[immutable-css]ss
```

The CLI exits with an error code if there are mutations, too. That way you can include the CLI as part of your CI:

```
some_ci_stuff && immutablecss vendor.css app.css && other_ci_stuff
```

## Dependencies

* <https://github.com/postcss/postcss>
* <https://github.com/tj/commander.js>
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
