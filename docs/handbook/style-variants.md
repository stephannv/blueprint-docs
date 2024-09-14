# Style Variants

Blueprint introduces a Style Variants extension, to simplify the management of
CSS class combinations, particularly for projects leveraging CSS frameworks such
as TailwindCSS. The process entails defining a schema for variants within the
component class to compile the final list of CSS classes.

This feature is inspired by [view_component-contrib](https://github.com/palkan/view_component-contrib)
gem.

!!! warning "Experimental"

    This feature is experimental and may undergo significant changes before
    stabilization, or it could be removed or separated from Blueprint entirely.

To define the variants, use `#style_builder` macro:
```crystal
class AlertComponent
  include Blueprint::HTML

  style_builder do
    base "alert"

    variants do
      type {
        info "alert-info"
        danger "alert-danger"
      }

      size {
        xs "text-xs p-2"
        md "text-md p-4"
        lg "text-lg p-6"
      }

      closable {
        yes "alert-closable"
      }

      special {
        yes "alert-special"
        no "alert-default"
      }
    end

    defaults type: :info, size: :md
  end

  def blueprint
    div(class: build_style(type: :danger, size: :lg, closable: true)) do
      "My styled alert component"
    end
  end
end

puts AlertComponent.new.to_s
```

Output:

```html
<div class="alert alert-danger text-lg p-6 alert-closable">
  My styled alert component
</div>
```

The available options are:

- `base` - defines the default styles for the component.
- `variants` - enables the creation of multiple style variations for the
component.
- `defaults` - specifies the default variant configurations for the component.

You can create your variant using the `build_style` macro, which can be applied
at both the instance level, as shown, or at the class level

```crystal
puts AlertComponent.build_style(type: :info, size: :xs, special: false)
# => "alert alert-info text-xs p-2 alert-default"
```

The `build_style` macro accepts a `NamedTuple` where the values are either
`Symbol` or `Bool`. Variants using `yes`/`no` can be applied with `true` or
`false`.

If you pass non-existent variants or invalid variant options, errors will be
raised:
```crystal
AlertComponent.build_style(shadow: :lg)
# Error: `shadow` variant was not defined inside style_builder `variants` block."

AlertComponent.build_style(type: :other)
# Error: `other` is an invalid option for type variant. Valid options are [:info, :danger]."
```
