Ruby
====

## Code style

### Indentation

* 2 spaces
* No tabs
* Add whitespaces around operators and `{}`

```ruby
# wrong
vehicle = {odometer: 10.0}

# correct
vehicle = { odometer: 10.0 }
```

```ruby
# wrong
vehicle_states.each {|vehicle_state| puts vehicle_state}

# correct
vehicle_states.each { |vehicle_state| puts vehicle_state }
```


```ruby
# wrong
def calculate_distance(odometer,ignition=true)
  # ...
end

# correct
def calculate_distance(odometer, ignition = true)
  # ...
end
```

* Don't add space around `()` or `[]`

```ruby
# wrong
speed_limits = [40,60,80]

# correct
speed_limits = [40, 60, 80]
```

```ruby
# wrong
filter( values )
filter (values)

# correct
filter(values)
```

* Add blank line between method definitions


```ruby
# wrong
def show_message
  puts 'Hello, world'
end
def sum(a, b)
  a + b
end

# correct
def show_message
  puts 'Hello, world'
end

def sum(a, b)
  a + b
end
```
