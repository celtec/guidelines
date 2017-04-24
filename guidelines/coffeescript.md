Coffeescript
============

## Indentation

* 2 spaces
* No tabs
* Add whitespaces around operators and `{}`

## Code Style

* Use `camelCase` to declare "classes", functions and variables
* Add spaces around `(...)` and before `->`

```coffee
# wrong
foo = (arg1,arg2)->

# correct
foo = (arg1, arg2) ->
```

Don't use parenthesis to define methods that take no arguments

```coffee
# wrong
bar = () ->

# correct
bar = ->
```

Use parenthesis on calling methods that take no arguments

```coffee
# wrong
bar

# correct
bar()
```

### Private

Methods that are intended to be `private` should begin with leading underscore.

```coffee
_privateMethod: ->
```

## Syntax

* Don't use `else` together with `unless`

```coffee
# wrong
unless driver
  checkCurrentDriver()
else
  registerDriver()
end

# correct
if driver
  registerDriver()
else
  checkCurrentDriver()
end
```

* Don't use `NOT` with `unless`

```coffee
# wrong
registerEvent() unless !hasTurnedOff()

# correct
registerEvent() if hasTurnedOff()
```
