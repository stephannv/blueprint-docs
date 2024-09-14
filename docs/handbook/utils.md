# Utils

## Overview

Blueprint provide some utility methods to help you build your HTML:

#### `#doctype`
Adds HTML 5 doctype declaration.


#### `#plain`
Writes plain text on HTML without tags.

#### `#whitespace`
Adds a simple whitespace to HTML.


#### `#comment`
Allows writing HTML comments.

#### `#raw`
Write content without escaping. You must pass a `Blueprint::SafeObject` to
this method. Learn more about this at [Safety](./safety.md) section.
**WARNING:** This must be used with great caution.
You should avoid using this method with any content that originates from an
untrusted source (eg. people on internet).

## Examples
```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    doctype

    comment "This is a comment"

    h1 do
      plain "Welcome"
      whitespace
      strong { "Jane Doe" }
    end

    raw safe("<script>alert('Nice Script!')</script>")
  end
end

ExamplePage.new.to_s
```

Output

```html
<!DOCTYPE html>

<!--This is a comment-->

<h1>
  Welcome <strong>Jane Doe</strong>
</h1>

<script>alert('Nice Script!')</script>
```
