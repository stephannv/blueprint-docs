# Multiple layouts

There are some ways to solve the problem of pages needing multiple layouts. The
[enveloping](../guides/enveloping.md) guide can introduce you the `#envelope`
method.


## Inheritance

Your pages can inherit from layout classes.

=== ":simple-crystal: MainLayout"

    ```crystal
    class MainLayout
      include Blueprint::HTML

      abstract def blueprint

      private def envelope(&)
        html do
          body do
            div class: "main-layout" do
              yield
            end
          end
        end
      end
    end
    ```

=== ":simple-crystal: AuthLayout"

    ```crystal
    class AuthLayout
      include Blueprint::HTML

      abstract def blueprint

      private def envelope(&)
        html do
          body do
            div class: "auth-layout" do
              yield
            end
          end
        end
      end
    end
    ```

=== ":simple-crystal: LoginPage"

    ```crystal
    class LoginPage < AuthLayout
      private def blueprint
        h1 { "Sign In" }
      end
    end
    ```

=== ":simple-crystal: HomePage"

    ```crystal
    class LoginPage < MainLayout
      private def blueprint
        h1 { "Welcome" }
      end
    end
    ```

## Generics

Your base page can have a generic type.

=== ":simple-crystal: BasePage"

    ```crystal
    class BasePage(T)
      include Blueprint::HTML

      abstract def blueprint

      private def envelope(&)
        render(T.new) do
          yield
        end
      end
    end
    ```

=== ":simple-crystal: LoginPage"

    ```crystal
    class LoginPage < BasePage(AuthLayout)
      private def blueprint
        h1 { "Sign In" }
      end
    end
    ```

=== ":simple-crystal: HomePage"

    ```crystal
    class LoginPage < BasePage(MainLayout)
      private def blueprint
        h1 { "Welcome" }
      end
    end
    ```

=== ":simple-crystal: MainLayout"

    ```crystal
    class MainLayout
      include Blueprint::HTML

      private def blueprint(&)
        html do
          body do
            div class: "main-layout" do
              yield
            end
          end
        end
      end
    end
    ```

=== ":simple-crystal: AuthLayout"

    ```crystal
    class AuthLayout
      include Blueprint::HTML

      private def blueprint(&)
        html do
          body do
            div class: "auth-layout" do
              yield
            end
          end
        end
      end
    end
    ```
