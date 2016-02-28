gobble-devnull
==============

This plugin is extremely simple: it will gobble (I don't have a better word,
sorry !) your input node. That means this input node won't contribute to the
final build.

This is especially useful for linters and tests. Indeed you don't want your test
files to clutter your final build, yet you want to run them and so they must be
part of your build at one moment.

Here is an example:
```javascript
const gobble = require('gobble');

const tests =
  gobble([
    gobble('src').moveTo('src'),
    gobble('test').moveTo('test')
  ])
  .observe('eslint')
  .transform('devnull');

module.exports = gobble([
  tests,
  gobble('src/root'),
  gobble([
    gobble('src/js'),
    gobble('src/libs'),
    gobble('src/jade').transform('jade-es6')
  ])
  .transform('babel')
  .transform('webpack', {
    entry: './entry.js',
    sourceMap: true
  })
]);
```
