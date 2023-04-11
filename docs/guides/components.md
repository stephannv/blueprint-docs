# Creating Components

## Overview

You can create reusable components using Blueprint, you just need to pass a
component instance to the `#render` method.

=== ":simple-crystal: AlertComponent"

    ```crystal
    class AlertComponent
      include Blueprint::HTML

      def initialize(@content : String, @type : String); end

      private def blueprint
        div class: "alert alert-#{@type}", role: "alert" do
          @content
        end
      end
    end
    ```

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Hello" }

        render AlertComponent.new(content: "My alert", type: "primary")
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Hello</h1>
    <div class="alert alert-primary" role="alert">
      My alert
    </div>
    ```

!!! warning "Pages vs. Views vs. Components"

    Please note that while these doc pages will use terms like views, pages and
    components to describe conceptual differences, in code all classes are
    implemented in the same way, there is no difference in practice.
    The `Blueprint::HTML` module must be included, which uses the `#blueprint`
    method to define an HTML structure.

## Passing Content

Sometimes you need to pass complex content that cannot be passed through a
constructor parameter. To accomplish this, the `blueprint` method needs to
receive a block (`&`) and yield it. Refactoring the previous Alert component
example:

=== ":simple-crystal: AlertComponent"

    ```crystal
    class AlertComponent
      include Blueprint::HTML

      def initialize(@type : String); end

      private def blueprint(&)
        div class: "alert alert-#{@type}", role: "alert" do
          yield
        end
      end
    end
    ```

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Hello" }

        render AlertComponent.new(type: "primary") do
          h4(class: "alert-heading") { "My Alert" }
          p { "Alert body" }
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Hello</h1>
    <div class="alert alert-primary" role="alert">
      <h4 class="alert-heading">My alert</h4>
      <p>Alert body</p>
    </div>
    ```

## Composing Components

Blueprint components can expose some predefined structure to its users. This
can be accomplished by defining public instance methods that can accept blocks
or not. Refactoring the previous Alert component example:


=== ":simple-crystal: AlertComponent"

    ```crystal
    class AlertComponent
      include Blueprint::HTML

      def initialize(@type : String); end

      private def blueprint(&)
        div class: "alert alert-#{@type}", role: "alert" do
          yield
        end
      end

      def title(&)
        h4(class: "alert-heading") { yield }
      end

      def body(&)
        p { yield }
      end
    end
    ```

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Hello" }

        render AlertComponent.new(type: "primary") do |alert|
          alert.title { "My Alert" }
          alert.body { "Alert body" }
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Hello</h1>
    <div class="alert alert-primary" role="alert">
      <h4 class="alert-heading">My alert</h4>
      <p>Alert body</p>
    </div>
    ```

## Registering Components

Blueprint has the `register_component` macro. It is useful to avoid writing the
fully qualified name of the component class. Instead writing something like
`render Views::Components::Forms::LabelComponent.new(for: "password")` you
could write just `label_component(for: "password")`.

!!! info "Blueprint::HTML::ComponentRegistrar"

    You need to require `"blueprint/html/component_registrar"` and include the `Blueprint::HTML::ComponentRegistrar` module to make `register_component` macro available.

=== ":simple-crystal: ExamplePage"

    ```crystal
    require "blueprint/html/component_registrar"

    class ExamplePage
      include Blueprint::HTML
      include Blueprint::HTML::ComponentRegistrar

      register_component :alert_component, AlertComponent

      private def blueprint
        h1 { "Hello" }

        alert_component(type: "primary") do |alert|
          alert.title { "My Alert" }
          alert.body { "Alert body" }
        end
      end
    end
    ```

=== ":simple-crystal: AlertComponent"

    ```crystal
    class AlertComponent
      include Blueprint::HTML

      def initialize(@type : String); end

      private def blueprint(&)
        div class: "alert alert-#{@type}", role: "alert" do
          yield
        end
      end

      def title(&)
        h4(class: "alert-heading") { yield }
      end

      def body(&)
        p { yield }
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Hello</h1>
    <div class="alert alert-primary" role="alert">
      <h4 class="alert-heading">My alert</h4>
      <p>Alert body</p>
    </div>
    ```
