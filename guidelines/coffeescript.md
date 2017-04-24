Coffeescript
============

## Indentation

* 2 spaces
* No tabs
* Add whitespaces around operators and `{}`

## Code Style

Add spaces around `(...)` and before `->`

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
  check_current_driver()
else
  register_driver()
end

# correct
if driver
  register_driver()
else
  check_current_driver()
end
```

* Don't use `NOT` with `unless`

```coffee
# wrong
register_event unless !turned_off?()

# correct
register_event if turned_off?()
```
