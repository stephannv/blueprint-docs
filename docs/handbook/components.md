# Components

## Overview

!!! note "Views vs. Components vs. Pages"

    Please note that while these doc pages will use terms like views, pages and
    components to describe conceptual differences, in code all classes are
    implemented in the same way, there is no difference in practice.
    The `Blueprint::HTML` module must be included, which uses the `#blueprint`
    method to define an HTML structure, and `#render` method accepts any class
    that included `Blueprint::HTML` module.

You can create reusable views using Blueprint, you just need to pass a component
instance to the `#render` method.

```crystal
class Alert
  include Blueprint::HTML

  def initialize(@content : String, @type : String); end

  private def blueprint
    div class: "alert alert-#{@type}", role: "alert" do
      @content
    end
  end
end

class Example
  include Blueprint::HTML

  private def blueprint
    h1 { "Hello" }

    render Alert.new(content: "My alert", type: "primary")
  end
end

Example.new.to_s
```

Output:

```html
<h1>Hello</h1>

<div class="alert alert-primary" role="alert">
  My alert
</div>
```

## Passing Content

Sometimes you need to pass complex content that cannot be passed through a
constructor parameter. To accomplish this, the component's `#blueprint` method
needs to yield a block. Refactoring the previous Alert component example:

```crystal
class Alert
  include Blueprint::HTML

  def initialize(@type : String); end

  private def blueprint(&)
    div class: "alert alert-#{@type}", role: "alert" do
      yield
    end
  end
end

class Example
  include Blueprint::HTML

  private def blueprint
    h1 { "Hello" }

    render Alert.new(type: "primary") do
      h4(class: "alert-heading") { "My Alert" }
      p { "Alert body" }
    end
  end
end

Example.new.to_s
```

Output:

```html
<h1>Hello</h1>

<div class="alert alert-primary" role="alert">
  <h4 class="alert-heading">My alert</h4>

  <p>Alert body</p>
</div>
```

!!! note "Yielding blocks"

    To define a method that receives a block, simply use yield inside it and the
    compiler will know. You can make this more explicit by declaring a dummy
    block parameter, indicated as a last parameter prefixed with ampersand (&).
    eg.
    ```crystal
    def blueprint(&)
      div { yield }
    end
    ```

## Composing Components

Blueprint components can expose some predefined structure to its users. This
can be accomplished by defining public instance methods that can yield blocks
or not. Refactoring the previous Alert component example:

```crystal
class Alert
  include Blueprint::HTML

  def initialize(@type : String); end

  private def blueprint(&)
    div class: "alert alert-#{@type}", role: "alert" do
      yield
    end
  end

  def title(&)
    h4(class: "alert-heading") { yield }
  end

  def body(&)
    p { yield }
  end
end

class Example
  include Blueprint::HTML

  private def blueprint
    h1 { "Hello" }

    render Alert.new(type: "primary") do |alert|
      alert.title { "My Alert" }
      alert.body { "Alert body" }
    end
  end
end
```

Output:

```html
<h1>Hello</h1>

<div class="alert alert-primary" role="alert">
  <h4 class="alert-heading">My alert</h4>

  <p>Alert body</p>
</div>
```
