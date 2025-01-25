# Intro

## What's Blueprint?
[Blueprint](https://github.com/stephannv/blueprint) is a framework for writing reusable and testable HTML templates in
plain Crystal, allowing an oriented object approach when building web views.

```crystal
class Alert
  include Blueprint::HTML

  private def blueprint
    div class: "alert alert-success" do
      h4(class: "alert-heading") { "Well done!" }
      span "Nice message here"
    end
  end
end

alert = Alert.new
alert.to_s
```

Output:

```html
<div class="alert alert-success">
  <h4 class="alert-heading">Well done!</h4>
  <span>Nice message here</span>
</div>
```

## Installation
In `shard.yml`, add:

```yaml
dependencies:
  blueprint:
    github: stephannv/blueprint
```

And run `shards install`.

## Why use Blueprint?

The benefits of using Blueprint to build HTML templates:

* **Pure Crystal**: One of the biggest benefits of using Crystal is its syntax,
and using Blueprint you can take advantage of this syntax when building HTML.

* **Oriented Object Programming**: Blueprint makes it easy to create
sustainable code, breaking web views in small pieces, encapsulating logic in
its specialized views classes, following best practices principles, eg.
Single responsability.

* **Safety**: All rendered content (eg. tags content and attributes
values) are escaped by default.

* **Fast enough & Memory efficient**: You can render significantly large views in nanoseconds or
microseconds with a low memory footprint, depending on your machine's performance.

* **Zero dependencies**: Blueprint relies solely on the standard Crystal library.

* **Phlex-friendly**: If you're used to working with Phlex in Ruby, you'll find
that Blueprint supports most of its syntax seamlessly.


## Support

If you run into any trouble, please [start a discussion](https://github.com/stephannv/blueprint/discussions), or [open an issue](https://github.com/stephannv/blueprint/issues)
if you  think you’ve found a bug.

## Alternatives

You can find Blueprint alternatives here:

- [awesome-crystal](https://github.com/veelenga/awesome-crystal?tab=readme-ov-file#html-builders)
- [shards.info](https://shards.info/tags/html)
- [shardsbox.org](https://shardbox.org/categories/HTML_Builders)
