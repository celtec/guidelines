Ruby
====

## Indentation

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

## Idioms

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

## Naming

### Classes

* Use `CamelCase`
* Use `UPCASE_SNAKE_CASE` for constants

### Methods

* Use `snake_case`
* Boolean return should end with `?`: Ex.: `valid?`

* Avoid types in names

```ruby
# wrong
vehicles_array = Vehicle.all

# correct
vehicles = Vehicle.all
```

## Syntax

* Always use `&&` and `||` for boolean expressions. Do not use `and` and `or` to avoid precedence issues

```ruby
# wrong
success! if valid? and persisted?

# correct
success! if valid? && persisted?
```

```ruby
# wrong
success! if sent? or registered?

# correct
success! if sent? || registered?
```

* Don't use `else` together with `unless`

```ruby
# wrong
unless driver
  check_current_driver
else
  register_driver
end

# correct
if driver
  register_driver
else
  check_current_driver
end
```

* Don't use `NOT` with `unless`

```ruby
# wrong
register_event unless !turned_off?

# correct
register_event if turned_off?
```

* Don't use `then` for multi-line `if/unless`

```ruby
# wrong
if turned_on? then
  register_event
end

# correct
register_event if turned_on?

# also correct
if turned_on?
  register_event
end
```

* Avoid `return` when not required

```ruby
# wrong
def valid?
  if vehicle.valid? && tracking_module.valid?
    return true
  else
    return false
  end
end

# correct
def valid?
  vehicle.valid? && tracking_module.valid?
end
```

* Avoid `self` when not required

```ruby
# wrong
def full_name
  self.first_name + ' ' + self.last_name
end

# correct
def full_name
  [first_name, last_name].join(' ')
end
```

* Use `_` for unused block parameters

```ruby
# wrong
speeds.map { |key, speed| register_speed(speed) }

# correct
speeds.map { |_, speed| register_speed(speed) }
```

* Use `attr_*` to define trivial methods

```ruby
# wrong
class Driver
  def initialize(name, license_number)
    @name = name
    @license_number = license_number
  end

  def name
    @name
  end

  def license_number
    @license_number
  end
end

# correct
class Driver
  attr_reader :name, :license_number

  def initialize(name, license_number)
    @name = name
    @license_number = license_number
  end
end
```

* If you're defining an empty class, do it in a single-line

```ruby
# wrong
class TruckDriver
end

class InvalidAxis < StandardError
end

# correct
class TruckDriver; end

class InvalidAxis < StandardError; end
```

* Only use `{ ... }` for single-line blocks, otherwise use `do/end`

```ruby
# wrong
vehicles.each do |vehicle|
  vehicle.update_last_state
end

# correct
vehicles.each { |vehicle| vehicle.update_last_state }

# also correct
vehicles.each(&:update_last_state)
```

```ruby
# wrong
vehicles.each { |vehicle|
  state = vehicle.update_last_state
  vehicle.register_speed(state.speed)
}

# correct
vehicles.each do |vehicle|
  state = vehicle.update_last_state
  vehicle.register_speed(state.speed)
end
```

## Data Syntax

* Always use new hash syntax when using Ruby >= 1.9x

```ruby
# wrong
{ :driver_name => 'Rubinho Barrichello' }

# correct
{ driver_name: 'Ayrton Senna' }
```

* Only use double-quoted string when there is interpolation

```ruby
# wrong
vehicle_model = "Ferrari 488 GTB"

# correct
vehicle_model = 'Ferrari 488 GTB'

# also correct
vehicle_model = "#{manufacturer_name} 488 GTB"
```

* Use `%w` to define array of strings

```ruby
# wrong
beaches = ['Campeche', 'Daniela', 'Solidão']

# correct
beaches = %w(Campeche Daniela Solidão)
```

## Standard library

* When using methods with `bang!` on strings, arrays and other enumerables, do not chain them

This is because many `bang!` methods return `nil` if no change happens ([for example, Array#reject!](http://ruby-doc.org/core-1.9.3/Array.html#method-i-reject-21)) and those mistakes are very hard to detect since it only happens in special circumstances.

```ruby
# wrong
vehicles = foreigner_vehicles.select { |v| v.is_a?(Ferrari) }.sort_by!(&:year)

# correct
vehicles = foreigner_vehicles.select { |v| v.is_a?(Ferrari) }
vehicles.sort_by!(&:year)
vehicles
```

* Use `tap` when memoizing instead of using `begin..end` block

```ruby
# wrong
def addresses
  @addresses ||= begin
    address = Services::Geolocate.locate(@data)

    raise 'Invalid coordinates for geofence' unless outside_geofence?(address)

    address
  end
end

# correct
def addresses
  @addresses ||= Services::Geolocate.locate(@data).tap do |address|
    raise 'Invalid coordinates for geofence' unless outside_geofence?(address)
  end
end
```

## Linting

For linting your Ruby code you can use our existing configs in all projects `.rubocop.yml`. To do that run:

```
$ gem install rubocop
$ rubocop
```
