# Overview

## What's Blueprint?
Blueprint is a framework for writing reusable and testable HTML templates in
plain Crystal, allowing an oriented object approach when building web views.

=== ":simple-crystal: Alert"

    ```crystal
    class Alert
      include Blueprint::HTML

      private def blueprint
        div class: "alert alert-success" do
          h4(class: "alert-heading") { "Well done!" }
          p { "Nice message here" }
        end
      end
    end
    ```

=== ":simple-html5: Output"

    ```html
    <div class="alert alert-success">
      <h4 class="alert-heading">Well done!</h4>
      <p>Nice message here</p>
    </div>
    ```

## Why use Blueprint?

The benefits of using Blueprint to build HTML templates:

* **Pure Crystal**: One of the biggest benefits of using Crystal is its syntax,
and using Blueprint you can take advantage of this syntax when building HTML.
* **Oriented Object Programming**: Blueprint makes it easy to create
sustainable code, breaking web views in small pieces, encapsulating logic in
its specialized views classes, following best practices principles, eg.
Single responsability.
