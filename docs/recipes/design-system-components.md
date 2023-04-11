# Design System Components

If you are using Blueprint to build a design system or a lot of base components,
eg. buttons, tabs, cards, alerts, etc. you will end up writing
fully-qualified class name when consuming components. For example:

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        render Structures::PageComponent.new do |page|
          page.title { "Example" }
          page.body do
            render Feedback::AlertComponent.new do |alert|
              alert.title { "Warning" }
              alert.body { "Your account couldn't be verified" }
            end
          end
        end
      end
    end
    ```

But you can take advantage of `Blueprint::HTML::ComponentRegistrar` module to
register your components, simplifying your life. You can create a module to
register the components and include it where needed.

=== ":simple-crystal: ComponentHelpers"

    ```crystal
    require "blueprint/html/component_registrar"

    module ComponentHelpers
      register_component :page_component, Structures::PageComponent
      register_component :alert_component, Feedback::AlertComponent
      # ... more components here
    end
    ```

=== ":simple-crystal: BasePage"

    ```crystal
    class BasePage
      include Blueprint::HTML
      include ComponentHelpers
    end
    ```

=== ":simple-crystal: ExamplePage"

    ```crystal
    class ExamplePage < BasePage
      private def blueprint
        page_component do |page|
          page.title { "Example" }
          page.body do
            alert_component do |alert|
              alert.title { "Warning" }
              alert.body { "Your account couldn't be verified" }
            end
          end
        end
      end
    end
    ```
