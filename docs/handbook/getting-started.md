# Getting Started

## Basic Usage

To start using Blueprint:

1. [Install Blueprint](../index.md#installation)
1. Require `"blueprint/html"`
1. Include `Blueprint::HTML` module in your class
1. Define a `#blueprint` method to write the HTML structure inside

```crystal
require "blueprint/html"

class ExamplePage
  include Blueprint::HTML

  private def blueprint
    h1 "Hello World"
  end
end
```

To render your view class it's necessary to instantiate it
and call `#to_s` method.

```crystal
example = Example.new
example.to_s # => "<h1>Hello World</h1>
```

## `Blueprint::HTML` is just a module
The `Blueprint::HTML` module just add helper methods to handle the HTML building
stuff and your class can have its own logic. For example, if you want pass data
to your view object:

```crystal
class ProfilePage
  include Blueprint::HTML

  def initialize(@user : User); end

  private def blueprint
    h1 @user.name

    admin_badge if admin?
  end

  private def admin_badge
    img src: "admin-badge.png"
  end

  private def admin?
    @user.role == "admin"
  end
end

user = User.new(name: "Jane Doe", role: "admin")
page = ProfilePage.new(user: user)
page.to_s
```

Output:

```html
<h1>Jane Doe</h1>
<img src="admin-badge.png">
```

It is possible even use structs instead of classes:

```crystal
struct Card
  include Blueprint::HTML

  private def blueprint
    div(class: "card") { "Hello" }
  end
end

# or using `record` macro
record Alert, message : String do
  include Blueprint::HTML

  private def blueprint
    div(class: "alert") { @message }
  end
end

card = Card.new
card.to_s # => <div class="card">Hello</div>

alert = Alert.new(message: "Hello")
alert.to_s # => <div class="alert">Hello</div>
```

## Passing content to views

If you want to pass content when rendering a view, you should define the
`#blueprint(&)` method and yield the block.

```crystal
class Alert
  include Blueprint::HTML

  private def blueprint(&)
    div class: "alert" do
      yield
    end
  end
end

alert = Alert.new
alert.to_s { "Hello World" }
```

Output:

```html
<div class="alert">
  Hello World
</div>
```
