# Safety

When using Blueprint all content and attribute values passed to elements and
components are escaped.

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        span { "<script>alert('hello')</script>" }

        input(class: "some-class\" onblur=\"alert('Attribute')")
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <span>&lt;script&gt;alert(&#39;hello&#39;)&lt;/script&gt;</span>

    <input class="some-class&quot; onblur=&quot;alert(&#39;Attribute&#39;)">
    ```
