# Safety

## Overview

When using Blueprint all content and attribute values passed to elements and
components are escaped by default.


```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    plain "<script>alert('DANGER!')</script>"

    comment "--><script>alert('DANGER!')</script><!--"

    span { "<script>alert('DANGER!')</script>" }

    input(class: "some-class\" onblur=\"alert('DANGER!')")
  end
end

ExamplePage.new.to_s
```

Output:

```html
&lt;script&gt;alert(&#39;DANGER!&#39;)&lt;/script&gt;

<!----&gt;&lt;script&gt;alert(&#39;DANGER!&#39;)&lt;/script&gt;&lt;!---->

<span>&lt;script&gt;alert(&#39;DANGER!&#39;)&lt;/script&gt;</span>

<input class="some-class&quot; onblur=&quot;alert(&#39;DANGER!&#39;)">
```

## Bypassing safety

To bypass escaping, you can use the `#raw` and `#safe` methods. The `#raw`
method appends content to buffer without escaping, while the `#safe` method
wraps the given object in a `Blueprint::SafeValue`, indicating to Blueprint that
the content should be rendered without escaping.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    div { safe("<script>alert('Nice Script!')</script>") }

    iframe srcdoc: safe("<div>Safe Content</div>")

    raw safe("<script>alert('Another Nice Script!')</script>")
  end
end

ExamplePage.new.to_s
```

Output

```html
<div>
  <script>alert('Nice Script!')</script>
</div>

<iframe srcdoc="<div>Safe Content</div>"></iframe>

<script>alert('Another Nice Script!')</script>
```
