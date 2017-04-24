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

## Views

## Assets

* Add custom stylesheets, images, js, coffescripts inside `app/assets`
* Use `vendor/assets` for third party libraries, such as `jQuery-UI`, `Underscore`, etc

## Testing

* Caution when mocking/stubbing your tests, overuse can hide errors
