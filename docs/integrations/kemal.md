# Kemal

Kemal is a lightning fast, super simple web framework. The framework built-in
view templating uses [ECR](https://crystal-lang.org/api/master/ECR.html) but
it can be integrated with Blueprint since any string returned from a route will
output to the browser.


=== ":simple-crystal: App"

    ```crystal
    require "kemal"
    require "blueprint/html"

    class HomePage
      include Blueprint::HTML

      private def blueprint
        h1 { "Hello World!" }
      end
    end

    get "/" do
      HomePage.new.to_html
    end

    Kemal.run
    ```

Run `crystal run src/your_app.cr` and visit `http://0.0.0.0:3000`. You should
see a big **Hello Word!**.

This [repo](https://github.com/stephannv/kemal_blueprint_demo_app) is a
proof-of-concept integrating Kemal + Blueprint, using a mini design system, page
layout, Tailwind and component registration.
