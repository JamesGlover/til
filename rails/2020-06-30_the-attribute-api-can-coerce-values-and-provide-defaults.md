# The attribute api can coerce values and provide defaults 

The rails attribute API can be used to add attributes to objects, or to extend
database backed attributes.

```ruby
  class Thing < ApplicationRecord
    attribute :price, :integer
  end
```

It allows for custom types:

```ruby
  class Thing < ApplicationRecord
    attribute :concentration, ConcentrationInNg.new
  end

  class ConcentrationInNg < ActiveRecord::Type::Value
    def cast(value_from_db_or_controller)
      Unit.new(value_from_do_or_controller, 'ng')
    end
  end
```

It allows for defaults:
```ruby
  attribute :my_string, :string, default: "new default"
```

Tempted to explore using it for better visibility of database columns in
the main codebase.

[API documentation](https://api.rubyonrails.org/classes/ActiveRecord/Attributes/ClassMethods.html)
