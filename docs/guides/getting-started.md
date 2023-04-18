# Getting started

## Basic Usage

After [installing the shard](../installation.md), you will need to follow three
steps to start using Blueprint:

1. Require `"blueprint/html"`
1. Include `Blueprint::HTML` module
1. Define a `#blueprint` method to write the HTML structure inside.

=== ":simple-crystal: ExamplePage"

    ```crystal
    require "blueprint/html"

    class ExamplePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Hello World" }
      end
    end
    ```

Your view class is now defined, to render it it's necessary to instantiate it
and call `#to_html` method.

=== ":simple-crystal: Main"

    ```crystal
    example = Example.new
    example.to_html # => "<h1>Hello World</h1>
    ```

## Blueprints Are Just POCOs
You are using just Plain Old Crystal Objects (POCOs). The `Blueprint::HTML` just
add helper methods to handle the HTML building stuff and your class can have its
own logic. For example, if you want pass data to your view object:

=== ":simple-crystal: ProfilePage"

    ```crystal
    class ProfilePage
      include Blueprint::HTML

      def initialize(@user : User); end

      private def blueprint
        h1 { @user.name }

        admin_badge if admin?
      end

      private def admin_badge
        img src: "admin-badge.png"
      end

      private def admin?
        @user.role == "admin"
      end
    end
    ```

=== ":simple-crystal: Main"

    ```crystal
    user = User.new(name: "Jane Doe", role: "admin")
    page = ProfilePage.new(user: user)
    page.to_thml
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Jane Doe</h1>
    <img src="admin-badge.png">
    ```

It is possible even use structs instead of classes:

=== "main.cr"

    ```crystal
    struct Card
      include Blueprint::HTML

      private def blueprint
        div(class: "card") { "Hello" }
      end
    end

    # or using `record` macro
    record Alert, message : String do
      include Blueprint::HTML

      private def blueprint
        div(class: "alert") { @message }
      end
    end

    card = Card.new
    card.to_html # => <div class="card">Hello</div>

    alert = Alert.new(message: "Hello")
    alert.to_html # => <div class="alert">Hello</div>
    ```

## Passing Content

If you want to pass content when rendering a blueprint, you should define the
`#blueprint(&)` method and yield the block.

=== ":simple-crystal: Alert"

    ```crystal
    class Alert
      include Blueprint::HTML

      private def blueprint(&)
        div class: "alert" do
          yield
        end
      end
    end
    ```

=== ":simple-crystal: Main"

    ```crystal
    alert = Alert.new
    alert.to_html { "Hello World" }
    ```

=== ":simple-html5: Output"

    ```html
    <div class="alert">
      Hello World
    </div>
    ```
