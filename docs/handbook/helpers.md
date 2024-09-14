# Helpers

## Overview

Blueprint provide some helper methods to help you build your HTML.

!!! note "Utils vs Helpers"

    The key difference between the utility methods described in
    [Utils](./utils.md) and these  helper methods is that utility methods append
    content to the buffer, while helper methods simply return values.

#### `#safe`
Wraps the given object in a `Blueprint::SafeValue`, indicating to Blueprint that
the content should be rendered without escaping. Learn more about this at [Safety](./safety.md).

```crystal
class ExamplePage
  include Blueprint::HTML

  def blueprint
    div { "<script>Nice script!</script>" }

    div { safe("<script>Nice script!</script>") }
  end
end

ExamplePage.new.to_s
```

Output:

```html
<div>&lt;script&gt;Nice script!&lt;/script&gt;</div>

<div><script>Nice script!</script></div>
```


#### `#tokens` (Experimental)
Allows to build classes based on conditionals. It receives a `NamedTuple` where
the keys are method names and the values are strings that will be returned if
the methods return truthy values.

```crystal
class UserComponent
  include Blueprint::HTML

  def blueprint
    h1 class: tokens(admin?: "is-admin", moderator?: "is-moderator") do
      "Jane Doe"
    end
  end

  def admin?
    false
  end

  def moderator?
    true
  end
end

UserComponent.new.to_s
```

Output:

```html
<h1 class="is-moderator">
  Jane Doe
</h1>
```
