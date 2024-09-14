# Attributes

## Overview

HTML tag methods (eg. `#h1`, `#meta`, `#div`, etc), accepts named arguments as
parameter, so it is possible to pass any attributes to an HTML element.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    div x: 1, y: 2.5, foo: :bar do
      input "@blur": "doSomething", v_model: "user.name"
    end
  end
end

puts ExamplePage.new.to_s
```

```html
<div x="1" y="1.2" foo="bar">
  <input @blur="doSomething" v-model="user.name">
</div>
```

Note that Blueprint parses all attributes name replacing `_` by `-`.
So `v_model: "name"` will become `v-model="name"`.

## NamedTuple attributes

If you pass a `NamedTuple` attribute to some tag attribute, it will be
flattened with a dash between each level. This is useful for `data-*` and
`aria-*` attributes.


```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    div data: { id: 42, target: "#home" }, aria: { selected: "true" } do
      "Home"
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<div data-id="42" data-target="#home" aria-selected="true">
  Home
</div>
```

## Boolean attributes

If you pass `true` to some attribute, it will be rendered as a boolean HTML
attribute, in other words, just the attribute name will be rendered without the
value. If you pass `false` the attribute will not be rendered. If you want the
attribute value to be `"true"` or `"false"`, use `true` and `false` between
quotes.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    input required: true, disabled: false, x: "true", y: "false"
  end
end

ExamplePage.new.to_s
```

Output:

```html
<input required x="true" y="false">
```

## Array attributes

If you pass an Array as attribute value, it will be flattened, compacted (`nil`
values will be removed) and joined using `" "` as separator.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    div class: ["a", "b", nil, ["c", "d"]] do
      "Hello"
    end
  end
end
```

Output:

```html
<div class="a b c d">
  Hello
</div>
```
