# Utils

## Helper methods

Blueprint provide some utility methods to help you build your HTML:

* `#doctype` - adds HTML 5 doctype declaration
* `#plain` - writes plain text on HTML without tags
* `#whitespace` - adds a simple whitespace to HTML
* `#comment` - allows writing HTML comments

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        doctype

        comment { "This is an HTML comment" }

        h1 do
          plain "Hello"
          whitespace
          strong { "Jane Doe" }
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <!DOCTYPE html>

    <!--This is an HTML comment-->

    <h1>
      Hello <strong>Jane Doe</strong>
    </h1>
    ```

## Custom tags

You can register custom HTML tags using the `register_element` macro. The first
argument is the helper method and the second argument is an optional tag name.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      register_element :trix_editor
      register_element :my_button, "v-btn"

      private def blueprint
        trix_editor

        my_button(to: "#home") { "My button" }
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <trix-editor></trix-editor>

    <v-btn to="#home">My button</v-btn>
    ```

It is possible to use `register_void_element` and `register_empty_element` in
case if want you elements without closing tags or elements that doesn't accept
content.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      register_void_element :my_tag
      register_empty_element :my_empty_tag

      private def blueprint
        my_tag

        my_empty_tag
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <my-tag>

    <my-empty-tag></my-empty-tag>
    ```
