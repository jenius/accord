accord
======

A unified interface for compiled languages and templates in javascript.

[![npm](https://badge.fury.io/js/accord.png)](http://badge.fury.io/js/accord)
[![tests](https://travis-ci.org/jenius/accord.png?branch=master)](https://travis-ci.org/jenius/accord)
[![coverage](https://coveralls.io/repos/jenius/accord/badge.png?branch=master)](https://coveralls.io/r/jenius/accord?branch=master)
[![dependencies](https://david-dm.org/jenius/accord.png)](https://david-dm.org/jenius/accord)

### Why should you care?

There are two other libraries out there that already do this same thing, [consolidate.js](https://github.com/visionmedia/consolidate.js) and [transformers](https://github.com/ForbesLindesay/transformers). After looking over and using both of them, I decided to make this one anyway mainly because of **maintenance**. When creating an interface to many different languages, all of which are constantly changing, you need to be on top of maintenance, testing, and releases. Transformers is not well maintained or tested, which rules it out. Conslidate.js is a little better, but the maintenance at this point is mostly accepting pull requests from people who have changes, rather than actively keeping on top of it. TJ has a lot to do, I understand.

Compiling many different languages is a central component of [roots](http://roots.cx), and it needs a clean, well-managed, and tightly maintained and tested library that adapts to each supported language's interface. We (the maintainers of roots) are not comfortable forking and/or making pull requests into a library that we cannot feel 100% confident in, and so far we have not been able to find one that we are yet. So this is accord, a javascript templating interface you can feel confident in.

### Installation

`npm install accord`

### Usage

Although we are planning a CLI interface which will be awesome, right now accord exposes only a javascript API. Since some templating engines are async and others are not, accord keeps things consistent by returning a promise for any compilation task (using [when.js](https://github.com/cujojs/when)). Here's an example in coffeescript:

```coffee
fs = require 'fs'
accord = require 'accord'
jade = accord.load('jade')

# render a string
jade.render('body\n  .test')
  .catch(console.error.bind(console))
  .done(console.log.bind(console))

# or a file
jade.renderFile('./example.jade')
  .catch(console.error.bind(console))
  .done(console.log.bind(console))

# or compile a string to a function
# (only some to-html compilers support this, see below)
jade.compile('body\n  .test')
  .catch(console.error.bind(console))
  .done (res) -> console.log(res.toString())

# or a file
jade.compileFile('./example.jade')
  .catch(console.error.bind(console))
  .done (res) -> console.log(res.toString())

# compile a client-side js template
jade.compileClient('body\n  .test')
  .catch(console.error.bind(console))
  .done (res) -> console.log(res.toString())

# or a file
jade.compileFileClient('./example.jade')
  .catch(console.error.bind(console))
  .done (res) -> console.log(res.toString())

```

Docs below should explain the methods executed in the example above.

##### Accord Methods

- `accord.load(string, object)` - loads the compiler named in the first param, npm package with the name must be installed locally, or the optional second param must be the compiler you are after. The second param allows you to load the compiler from elsewhere or load an alternate version if you want, but be careful.

- `accord.supports(string)` - quick test to see if accord supports a certain compiler. accepts a string (name of compiler), returns a boolean.

##### Accord Adapter Methods

- `adapter.render(string, options)` - render a string to a compiled string
- `adapter.renderFile(path, options)` - render a file to a compiled string
- `adapter.compile(string, options)` - compile a string to a function
- `adapter.precompileFile(path, options)` - compile a file to a function
- `adapter.compileClient(string, options)` - compile a string to a client-side-ready function
- `adapter.compileFileClient(string, options)` - compile a file to a client-side-ready function
- `adapter.extensions` - array of all file extensions the compiler should match
- `adapter.output` - string, expected output extension
- `adapter.compiler` - the actual compiler, no adapter wrapper, if you need it

### Supported Languages

##### HTML
- [jade](http://jade-lang.com/)
- [ejs](https://github.com/visionmedia/ejs)
- [markdown](https://github.com/chjj/marked)
- mustache/hogan _(pending)_
- handlebars _(pending)_
- haml _(pending)_
- haml-coffee _(pending)_
- dust _(pending)_
- nunjucks _(pending)_
- underscore _(pending)_
- swig _(pending)_
- toffee _(pending)_

##### CSS
- [stylus](http://learnboost.github.io/stylus/)
- scss _(pending)_
- less _(pending)_

##### Javascript
- [coffeescript](http://coffeescript.org/)
- coffeescript-redux _(pending)_
- dogescript _(pending)_
- coco _(pending)_
- typescript _(pending)_

##### Minifiers
- [minify-js](https://github.com/mishoo/UglifyJS2)
- [minify-css](https://github.com/GoalSmashers/clean-css)
- [minify-html](https://github.com/kangax/html-minifier)
- csso _(pending)_

### Languages Supporting Precompile

Accord can also precompile templates into javascript functions for some languages, which is really useful for client-side rendering. Languages with precompile support are listed below. If you try to precompile a language without support for it, you will get an error.

- jade
- ejs _(pending)_
- underscore _(pending)_

We are always looking to add precompile support for more languages, but it can be difficult. Any contributions that help to expand this list are greatly appreciated!

### Adding Languages

Want to add more languages? We have put extra effort into making the adapter pattern structrue understandable and easy to add to and test. Rather than requesting that a language be added, please add a pull request and add it yourself! We are quite responsive and will quickly accept if the implementation is well-tested.

Details on running tests and contributing [can be found here](contributing.md)

### License

Licensed under [MIT](license.md)
