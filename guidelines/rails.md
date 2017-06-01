Rails
=====

## Configuration

* Use `config/initializers` directory to custom initialization files, always keeping separate file per subject
* Configurations applicables for all environments should go to `config/application.rb`

## Routes

When linking to routes inside application always use relative helpers: `*_path` instead `*_url`. Helpers with `*_url` should only be used when absolute path is really necessary.

## Controllers

Try to keep controllers' actions skinny, avoiding adding logic to it. Most of logic can be moved to a separate class to do this work.

### Reducing noise

```ruby
# wrong
class UsersController < ApplicationController
  def index
    @users = User.where(status: params[:status])
  end
end


# correct
class User < ActiveRecord::Base
  DEFAULT_STATUS = 'active'

  scope :by_status, -> (status) { where(status: status.presence || DEFAULT_STATUS) }
end

class UsersController < ApplicationController
  def index
    @users = User.by_status(params[:status])
  end
end
```

### Wrapping logic

```ruby
# wrong
class UsersController < ApplicationController
  def create
    role = params[:user][:role]

    if valid_role?(role)
      @user.role = role
      @user.accessible = load_accessible_fields
      @user.attributes = params[:user]

      if @user.save
        redirect_to(users_path, notice: t('messages.users.success.save'))
      else
        render :new
      end
    else
      redirect_to root_url
    end
  end
end

# correct
class UsersController < ApplicationController
  def create
    creator_service = Services::UserCreator.new(params[:user])

    if creator_service.call
      redirect_to(users_path, notice: t('messages.users.success.save'))
    else
      @user = creator_service.user

      render :new
    end
  end
end
```

## Models

* Don’t be afraid of introducing non-Active Record objects to your model directory.
* Avoid using `default_scope`. When you use `default_scope` you are defining that new objects and its select queries will always have such scope. `default_scope` is dangerous because it is a hidden behavior, some people will not expect a scope applied in a model. Prefer explicitly calling your scopes to quickly give your reader your intention.
* Group macro-style methods (`has_many`, `validates`, etc) and organize the order of these groups to be consistent in all your model files. The following example show you how you can organize your models:

```ruby
class User < ActiveRecord::Base
  # Constants
  AVAILABLE_ROLES = %w(maintenance operator client mobile)

  # Attributes
  attr_accessor :password

  # Associations
  belongs_to :operator
  belongs_to :client

  has_many :messages, dependent: :delete_all

  # Enums
  enum status: [:active, :expired, :blocked]

  # Scopes
  scope :maintenances, -> { where(role: 'maintenance') }

  # Validations
  validates_inclusion_of :role, in: AVAILABLE_ROLES
  validates :name, :username, :role, presence: true

  # Callbacks
  before_save :set_client_operator

  # Class methods
  def self.schedule_expiration(expiration_date)
    # ...
  end

  # Instance methods
  def expired?
    !expiration_date.nil? && expiration_date < Time.now
  end
end
```

* Prefer to use `validates` instead `validates_*`:

```ruby
# wrong
validates_presence_of :email, :username

# correct
validates :email, :username, presence: true
```

* Prefer `scopes` instead of class methods ([see details](http://blog.plataformatec.com.br/2013/02/active-record-scopes-vs-class-methods/)):

```ruby
# wrong
def self.by_token(token)
  return nil unless token

  find_by_token(token)
end

# correct
scope :by_token, -> (token) { where(token: token) }
```

## Views

* Instead accessing your models methods directly in views, prefer to define an instance variable in controllers

```ruby
# wrong
<%= form_for @user do |f| %>
  <%= f.text_field :name %>
  <%= f.text_field :username %>
  <%= f.collection_check_boxes :vehicle_ids, Vehicle.accessible, :id, :registration_plate %>

  <%= f.submit %>
<% end %>

# correct
class VehiclesController < ApplicationController
  def new
    @vehicles = Vehicle.accessible
  end
end

<%= form_for @user do |f| %>
  <%= f.text_field :name %>
  <%= f.text_field :username %>
  <%= f.collection_check_boxes :vehicle_ids, @vehicles, :id, :registration_plate %>

  <%= f.submit %>
<% end %>
```

* Avoid adding too much HTML in your view helper methods

```ruby
# correct
def icon(icon_name)
  content_tag(:i, class: "fa-#{icon_name}")
end
```

## Assets

* Add custom stylesheets, images, js, coffescripts inside `app/assets`
* Use `vendor/assets` for third party libraries, such as `jQuery-UI`, `Underscore`, etc

## Testing

* Caution when mocking/stubbing your tests, overuse can hide errors

## YAML

Its not necessary to use quotes on defining keys with hyphen `-`:

```yaml
# wrong
"pt-BR":
  alert: Alerta

# correct
pt-BR:
  alert: 'Alerta'
```

Use single-quotes for messages that doesn't require interpolation, then double quotes for those who require:

```yaml
# wrong
pt-BR:
  alert: Alerta
  hello: 'Hello %{name}'

# correct
  alert: 'Alerta'
  hello: "Hello %{name}"
```
