# Enveloping

By overriding the `#envelope` method, you can create a wrapper around your view
content. This is useful when defining layouts for pages, for example. Don't
forget to yield a block inside `#envelope` method.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    h1 { "Home" }
  end

  private def envelope
    html do
      body do
        yield
      end
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<html>
  <body>
    <h1>Home</h1>
  </body>
</html>
```

With `#envelope` method you can create some flexible layouts for your pages. For
example, you can use inheritance to have multiple layouts on your application.

```crystal
class MainLayout
  include Blueprint::HTML

  private def envelope
    html do
      body do
        div class: "main-layout" do
          yield
        end
      end
    end
  end
end

class AuthLayout
  include Blueprint::HTML

  private def envelope
    html do
      body do
        div class: "auth-layout" do
          yield
        end
      end
    end
  end
end

class HomePage < MainLayout
  private def blueprint
    h1 { "Welcome" }
  end
end

class LoginPage < AuthLayout
  private def blueprint
    h1 { "Sign In" }
  end
end

HomePage.new.to_s

LoginPage.new.to_s
```

HomePage output:

```html
<html>
  <body>
    <div class="main-layout">
      <h1>Welcome</h1>
    </div>
  </body>
</html>
```

LoginPage output:

```html
<html>
  <body>
    <div class="auth-layout">
      <h1>Sign In</h1>
    </div>
  </body>
</html>
```
