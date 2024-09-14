# Builder

`Blueprint::HTML` module provides the `.build` method for those cases when you
don't need or don't want to create a class or struct to write HTML.

```crystal
html = Blueprint::HTML.build do
  h1 { "Hello" }
  div do
    span { "World" }
  end
end

puts html
```

Output:

```html
<h1>Hello</h1>

<div>
  <span>World</span>
</div>
```
