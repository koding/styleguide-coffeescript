Koding CoffeeScript Style Guideline
=======================

### General

- Use 2 __spaces only__ for indentation. Never use tabs. At all!
- Keep your lines under __80__ characters
- Do not include any __trailing whitespace__ on any lines.

### Optional Commas

- Avoid use of commas on multiline object and array definitions.

```coffee
# No
arr = [
  'foo',
  'bar'
]

obj =
  foo: bar,
  baz: qux

# Yes
arr = [
  'foo'
  'bar'
]

obj =
  foo : bar
  baz : qux
```


### Blank Lines

- Leave 2 blank lines at the end of the file.

- Use 1 blank line between class decleration and the first method (generally constructor)

```coffee
# No
class Foo extends Bar
  constructor: (options = {}, data) -> # no space at all

    @options = options
    @data    = data

# Yes
class Foo extends Bar
# 1 \n
  constructor: (options = {}, data) ->

    @options = options
    @data    = data
```

- Use exactly 2 blank lines between method/function definitions

```coffee
# No
class Foo extends Bar

  constructor: (options = {}, data) ->

    @options = options
# 1 \n
  getOptions: -> @options


# Yes
class Foo extends Bar

  constructor: (options = {}, data) ->

    @options = options
# 1 \n
# 2 \n
  getOptions: -> @options


# Yes
doSomething = ->

  bar()
  baz()
# 1 \n
# 2 \n
doSomething = ->

  qux()
  fn()


```

- Use 1 blank line after definition of the method/function

```coffee
# No
doSomething = ->
  bar()
  baz()


# Yes
doSomething = ->

  foo()
  bar()
```

### Formatting

- Use at least one space before an assignment operator.
- Use **only** one space after an assignment operator.
- Use reasonable aligning for expression symbols _(e.g `=`, `:`)_. Do not try to align everything, as it's making it hard to move the lines. Try to use plugins for this: Tabularize for Vim, Alignment for Sublime Text

```coffee
# No
x= 1
y =2
z=3

# Yes
x = 1
y = 2
z = 3

# No
x            = 1
y            = 2
longVariable = 'string'

# Yes
x = 1
y = 2

longVariable = 'string'

# No
obj =
  var: 1
  short: 2
  longVariable: 3

obj            =
  var          : 1
  short        : 2
  longVariable : 3


# Yes
obj =
  var          : 1
  short        : 2
  longVariable : 3
```

- Follow idiomatic CoffeeScript practises for expressions, assignments, booleans etc.

```coffee
# No
obj  = obj || {}
bool = condition && otherCondition
isWrong = !right
expression = first == second
expression = first != second
bool = true
bool = false

# Yes
obj      or= {}
bool       = condition and otherCondition
isWrong    = not right
expression = first is second
expression = first isnt second
bool       = yes # `on` depending on the context (e.g `isLoggedIn = yes` vs `state = on`)
bool       = no  # `off` ^^
```



### Modules

We are using `CommonJS` module imports with `Browserify`.

- Each require statement needs to be on its own line

```coffee
_      = require 'underscore'
KDView = require 'kdf/view'
```

Require statements should follow the following order:

1 - 3rd Party Library imports
2 - Internal library/framework imports
3 - Application specific imports


### Parantheses, Curlies, Brackets

- Omit curly brackets for multiline object definition

```coffee
# No
obj = {
  foo : bar
  baz : qux
}

# Yes
obj =
  foo : bar
  baz : qux
```

- Use curly brackets for single line object definition

```coffee
# No
obj = foo: bar, baz: qux

# Yes
obj = { foo: bar, baz: qux }
```

- Omit paranthesis from the last function call of chain

```coffee
# No
foo('bar')
foo().bar('baz', 'qux')

# Yes
foo 'bar'
foo().bar 'baz', 'qux'
```

- Group only the first method in chains with `Lisp-y` way.

```coffee
# No
foo('bar').baz()
foo(bar('baz')).qux()
(foo (bar 'baz'))
((foo 'bar').baz 'qux').etc()
(foo 'bar').baz()
(foo 'bar').baz('qux').etc()

# Yes
foo().bar('baz').qux()
foo().bar 'baz'
foo('bar').baz('qux').etc()
```

- Multiline chains

```coffee
# No
foo('bar').baz()
  .qux()

# Yes
foo 'bar'
  .baz()
  .qux()
```


# Strings

- Use string interpolations instead of string concatenation.

```coffee
# No
str = 'This string has ' + variables + 'inside.'
str += ' And this is cool.'

# Yes
str = "This string has #{variables} inside."
str = "#{str} And this is cool."
```

- Use single quotes if there is no string interpolation.

```coffee
# No
str = "This is a string."

# Yes
str = 'This is a string.'
```


# Conditionals

- Use existential operator `arg?` in places where you really want to check if a value exists on that variable, and you are not sure about the type. Otherwise do not use existential operator, do the check against variable itself.

```coffee

doSomething = (obj) ->

  doSomeAsyncStuff obj, (err, result) ->

    # we are not sure about the type of err
    # but we know that if it's not `undefined`
    # or `null` we need to stop execution.
    return console.error err  if err?

    # we know that it will be some kind of
    # object, either plain object or an array.
    # (The result is almost always like this from
    # our backend requests.)
    doSomethingWithResult result  if result

```

- Use the existential operator on functions when they need to be called, but only if they exist. This syntax automatically checks the type of the function, avoiding those pesky `foo is not a function` errors.

```coffee
# No
callback foo  if callback
callback()    if callback

# Yes
callback? foo
callback?()
```

- Never use single line `if/then/else` statements. Instead use 3 line version of it.

```coffee
# No
if condition then foo() else bar()

# Yes
if condition
then foo()
else bar()
```

- Always use `if/else` over `unless/else`. Never use `unless/else`

```coffee
# No
unless no
  # do something
else
  # ...

# Yes
if yes
  # do something
else
  # ...
```

- Use 2 spaces before post conditionals

```coffee
# No
foo = 'bar' if condition # only one space

doSomething = ->

  return unless condition

# Yes
foo = 'bar'  if condition # 2 spaces

doSomething = ->

  return  unless condition # 2 spaces
```

- Use `switch` over `if/else if` for 1 line multi conditions. Align `then` statements if single line.

```coffee
# No
if condition then doSomething()
else if anotherCondition then doSomethingElse()
else if otherCondition then doOtherThing()
else defaultFn()

if condition is 'foo' then doSomething()
else if condition is 'bar' or condition is 'baz' then doSomethingElse()
else if condition is 'qux' then doOtherThing()
else defaultFn()

# Yes
switch
  when condition        then doSomething()
  when anotherCondition then doSomethingElse()
  when otherCondition   then doOtherThing()
  else defaultFn()

switch condition
  when 'foo'        then doSomething()
  when 'bar', 'baz' then doSomethingElse()
  when 'qux'        then doOtherThing()
  else defaultFn()
```


### Functions

- Don't use parens functions that has empty arguments list

```coffee
# No
doSomething = () ->

# Yes
doSomething = ->
```

- Use 1 space between closing parenthesis of arguments list and function arrow.

```coffee
# No
doSomething = (foo, bar, rest...)->

# Yes
doSomething = (foo, bar, rest...) ->
```

- Use 1 space after comma between arguments.

```coffee
# No
doSomething = (foo,bar,rest...)->

# Yes
doSomething = (foo, bar, rest...) ->
```

- Omit curly brackets if argument is a multiline object

```coffee
KDView = require 'kdf/view'

# No
new KDView {
  cssClass : 'bar'
  partial  : 'View text'
}

# Yes
new KDView
  cssClass : 'bar'
  partial  : 'View text'
```

- Use early returns over big `if/else` blocks, to avoid nesting.

```coffee
# No
doSomething = (state) ->

  if state
    # do something
  else
    return yes

# Yes
doSomething = (state) ->

  return yes  unless state

  # do something
```

- Omit `return` keyword __only__ for 1 line functions. Use `return` every where else.

```coffee
# No
doSomething = ->

  result = doThing()
  doOtherThing()

  result

class Foo

  getOptions: -> return @options

# Yes
doSomething = ->

  result = doThing()
  doOtherThing()

  return result

class Foo

  getOptions: -> @options

```

- Write method definition and method body on the same line if method body contains only one line. Only exception is when it is against `80 characters per line rule`.

```coffee
# No
isGreater = (foo, bar) ->

  return foo > bar

someKindOfMethodWithLongName = (foo, bar) -> [foo, bar].map (arg) -> anotherMethod arg

# Yes
isGreater = (foo, bar) -> foo > bar

someKindOfMethodWithLongName = (foo, bar) ->

  return [foo, bar].map (arg) -> anotherMethod arg
```

- Do not use `arguments`, use splat (`args...`) operator instead.

```coffee
# No
class Foo

  doSomething: ->

    doSomethingElseWith arguments

# Yes
class Foo

  doSomething: (args...) ->

    doSomethingElseWith args...

```

- Do not destruct properties on arguments list. Instead destruct necessary arguments inside function body.

```coffee
# No
doSomething = ({foo, bar, baz}, qux) ->
  # do something with foo, bar, baz

# Yes
doSomething = (obj, qux) ->

  { foo, bar, baz } = obj
  # do something with foo, bar, baz
```


### Classes

In Koding we wrote most of the codes with classes.

- Group helper/private methods in a private object called `helper`

```coffee
# NO
class Foo extends Bar

  doSomething        = (foo, bar) -> "#{foo} and #{bar}"
  duplicateSomething = (something) -> "#{something}#{something}"

  constructor: ->
    something   = doSomething 'foo', 'bar'
    @duplicated = duplicateSomething something

# YES
class Foo Extends Bar

  constructor: ->

    { doSomething, duplicateSomething } = helper

    something   = doSomething 'foo', bar
    @duplicated = duplicateSomething something


  helper =
    doSomething: (foo, bar) -> "#{foo} and #{bar}"
    duplicateSomething: (something) -> "#{something}#{something}"

```

- Use static methods or even private methods for methods that don't depend on `this` context. Do not use instance methods for those kind of methods.

```coffee
# No
class Foo extends Bar

  constructor: (options = {}, data) ->

    { foo, bar } = options

    eligible = @isEligible foo, bar

  # There is no `this` usage in this method
  # So there is no need for it to be an instance
  # method.
  isEligible: (foo, bar) -> foo and bar

# Yes
class Foo extends Bar

  @isEligible: (foo, bar) -> foo and bar

  constructor: (options = {}, data) ->

    { foo, bar } = options

    eligible = Foo.isEligible foo, bar # better
    eligible = helper.isEligible foo, bar # even better, it's just a function

  helper =
    isEligible: (foo, bar) -> foo and bar

```

- Define different types of methods in the following order:

  1 - Define `static` methods
  2 - Define `instance` methods
  3 - Define `helper` methods

```coffee
class Foo extends Bar

  # Static Methods
  @staticMethod: -> log 'static method'


  # Instance Methods
  constructor: (options = {}, data) ->

    @options = options
    @data    = data


  getOptions: -> @getOptions


  # Helper methods
  helper =
    transformOptions: (options) -> someTransformation options


```

- Use shorthand syntax for accesing prototype properties. Use direct access when dealing the `prototype` object itself.

```coffee
# No
slice      = Array.prototype.slice
arrayProto = Array::

# Yes
slice      = Array::slice
arrayProto = Array.prototype
```

- Use `@property` instead of `this.property`. Avoid using standalone `@`.

```coffee
class Foo extends Bar

  constructor: (options = {}, data) ->

    # No
    this.options = options

    # Yes
    @options = options

  # No
  doSomething: ->
    # do things
    # ...

    return @

  # Yes
  doSomething: ->
    # do things
    # ...

    return this

```

- Be careful with __fat arrows__. As they produce extra code, and tries to bind `this` into that method, if you know that you will not use context in that method, DO NOT USE fat arrows.

```coffee
class Foo extends Bar

  doSomething: (obj) ->

    # No
    # There is no access to the instance
    # or this, so there is no point using fat arrow here.
    doAsyncStuff obj, (err, result) => KD.utils.stringify result

    # Yes
    # Using thin arrow does the job well enough.
    doAsyncStuff obj, (err, result) -> KD.utils.stringify result


```
