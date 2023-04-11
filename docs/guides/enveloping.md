# Enveloping

By overriding the `#envelope(&)` method, you can create a wrapper around
blueprint content. This is useful when defining layouts for pages, for example.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Home" }
      end

      private def envelope(&)
        html do
          body do
            yield
          end
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <html>
      <body>
        <h1>Home</h1>
      </body>
    </html>
    ```
