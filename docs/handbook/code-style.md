# Code style

If you are new to Crystal, know that blocks can be passed using `do ... end` or
`{ ... }`. All of these are equivalent:

```crystal

# With `do ... end` blocks
private def blueprint
  html lang: "pt-BR" do
    head do
      title do
        "Blueprint"
      end
    end

    body do
      h1 class: "heading" do
        "Hello World"
      end
    end
  end
end

# With `{ ... }` blocks
private def blueprint
  html lang: "pt-BR" {
    head {
      title {
        "Blueprint"
      }
    }

    body {
      h1 class: "heading" {
        "Hello World"
      }
    }
  }
end
```

It's a personal or team preference, you can stick to one style or mix two
styles (eg. Use `do ... end` for multiline blocks and `{ ... }` for inline
blocks). But note that in Crystal the difference between using `do ... end` and
`{ ... }` is that `do ... end` binds to the left-most call, while `{ ... }`
binds to the right-most call:

With `do ... end` blocks:
```crystal
private def blueprint
  render Alert.new do
    "Hello World"
  end
end

# The above is same as
private def blueprint
  render(Alert.new) do
    "Hello World"
  end
end
```

While using `{ ... }` blocks:
```crystal
private def blueprint
  render Alert.new {
    "Hello World"
  }
end

# The above is same as
private def blueprint
  render(Alert.new {
    "Hello World"
  })
end
```

The example above will raise an error `Error: 'Alert.new' is not expected to
be invoked with a block, but a block was given`, to fix this you need to add
parentheses to `render` call when using with `{ ... }`.

```crystal
private def blueprint
  render(Alert.new) {
    "Hello World"
  }
end
```

You can pass content to tags either through the first positional argument or via
a block. All of the following are valid:

```crystal
private def blueprint
  h1 "Hello World"

  h1 { "Hello World" }

  h1 do
    "Hello World"
  end

  h1 "Hello World", class: "heading"

  h1(class: "heading") { "Hello World" }

  h1(class: "heading") do
    "Hello World"
  end
end
```
