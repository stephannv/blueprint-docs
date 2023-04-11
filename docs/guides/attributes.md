# Attributes

## Overview

HTML element methods, eg. `#h1`, `#meta`, `#div`, etc, accepts a NamedTuple as
parameter, so it is possible to pass any attributes to an HTML element.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        div x: 1, y: 2.5, foo: :bar do
          input "@blur": "doSomething", v_model: "user.name"
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <div x="1" y="1.2" foo="bar">
      <input @blur="doSomething" v-model="user.name">
    </div>
    ```

!!! info "Attribute name conversion"

    Note that Blueprint parses all attributes name replacing `_` by `-`

## NamedTuple attributes

If you pass a NamedTuple attribute to some element attribute, it will be
flattened with a dash between each level. This is useful for `data-*` and
`aria-*` attributes.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        div data: { id: 42, target: "#home" }, aria: { selected: "true" } do
          "Home"
        end
      end
    end
    ```

=== ":simple-html5: Output"

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

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        input required: true, disabled: false, x: "true", y: "false"
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <input required x="true" y="false">
    ```

## Array attributes

If you pass an Array as attribute value, it will be flattened and joined using
`" "` as separator.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        div class: ["a", "b", ["c", "d"]] do
          "Hello"
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <div class="a b c d">
      Hello
    </div>
    ```
