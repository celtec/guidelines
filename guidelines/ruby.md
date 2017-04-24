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

* Don't add a blank line after class, method, spec definition or before respective `end`

```ruby
# wrong
class TrackingModule

  def initialize(data)
    # ...
  end

end

# correct
class TrackingModule
  def initialize(data)
    # ...
  end
end
```

* Add blank line around `private` or `protected` definition

```ruby
# wrong
class TrackingModule
  def initialize(data)
    # ...
  end
  private

  def locate
    # ...
  end
end

# correct
class TrackingModule
  def initialize(data)
    # ...
  end

  private

  def locate
    # ...
  end
end
```

* Indent `when` and `else` at same level of `case`

```ruby
# correct
case speed
when 1..20 then 'Low gear'
when 21..40 then 'Low speed'
when 41..60 then 'Medium speed'
else 'High speed'
end

# alternative correct
case
when speed < 80
  register_speed(speed)
when speed > 80
  send_speed_danger_alert(speed)
end
```

* When there are multiple arguments

```ruby
# wrong
register_state(speed: 50, ignition: false)

# correct
register_state(
  speed: 50,
  ignition: false
)
```

* When there are multiple methods being chained

```ruby
# wrong
Vehicle.states
  .where(ignition: true)
  .order(created_at: :asc)
  .limit(10)

# correct
Vehicle.states.where(ignition: true).
               order(created_at: :asc).
               limit(10)
```

### Idioms

Only use parenthesis when there are params in method definition

```ruby
# wrong
def save()
  # ...
end

# correct
def save
  # ...
end

# also correct
def save(validate = true)
end
```

### Naming

#### Classes

* Use `CamelCase`
* Use `UPCASE_SNAKE_CASE` for constants

#### Methods

* Use `snake_case`
* Boolean return should end with `?`: Ex.: `valid?`

* Avoid types in names

```ruby
# wrong
vehicles_array = Vehicle.all

# correct
vehicles = Vehicle.all
```

