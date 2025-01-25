# Utils

## Overview

Blueprint provide some helper methods to help you build your HTML.

!!! note "Utils vs Helpers"

    The key difference between the utility methods described here and the
    helper methods described in [Helpers](./helpers.md) is that helper methods
    append content to the buffer, while utility methods just return values
    without appending anything to the buffer.

#### `#safe`
Wraps the given object in a `Blueprint::SafeValue`, indicating to Blueprint that
the content should be rendered without escaping. Learn more about this at
[Safety](./safety.md).

```crystal
class Example
  include Blueprint::HTML

  def blueprint
    div { "<script>Nice script!</script>" }

    div { safe("<script>Nice script!</script>") }
  end
end

Example.new.to_s
```

Output:

```html
<div>&lt;script&gt;Nice script!&lt;/script&gt;</div>

<div><script>Nice script!</script></div>
```

#### `#escape_once`
Returns an escaped version of given content without affecting existing escaped
entities.

```crystal
class Example
  include Blueprint::HTML

  def blueprint
    div { escape_once("1 < 2 &amp; 3") }

    div { escape_once("&lt;&lt; Accept & Checkout") }
  end
end

Example.new.to_s
```

Output:

```html
<div>1 &lt; 2 &amp; 3</div>

<div>&lt;&lt; Accept &amp; Checkout</div>
```
