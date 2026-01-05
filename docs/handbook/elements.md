# Elements

## Overview

All non-deprecated HTML elements are available as instance methods on
`Blueprint::HTML`. The tag methods accept a `NamedTuple` or `Hash`, which is 
converted into HTML attributes.

```crystal
class Example
  include Blueprint::HTML
  
  private def blueprint
    span { "Hello!" }

    h1 class: "heading" do
      "Hello!"
    end
  end
end

Example.new.to_s
```

Output:
```html
<span>Hello!</span>

<h1 class="heading">Hello!</h1>
```

## Standard elements

The avaiable element methods are:

- **Standard elements**: `#a`, `#abbr`, `#address`, `#article`, `#aside`, `#audio`, `#b`, `#bdi`, `#bdo`, `#blockquote`, `#body`, `#button`, `#canvas`, `#caption`, `#cite`, `#code`, `#colgroup`, `#data`, `#datalist`, `#dd`, `#del`, `#details`, `#dfn`, `#dialog`, `#div`, `#dl`, `#dt`, `#em`, `#fieldset`, `#figcaption`, `#figure`, `#footer`, `#form`, `#h1`, `#h2`, `#h3`, `#h4`, `#h5`, `#h6`, `#head`, `#header`, `#hgroup`, `#html`, `#i`, `#ins`, `#kbd`, `#label`, `#legend`, `#li`, `#main`, `#map`, `#mark`, `#menu`, `#meter`, `#nav`, `#noscript`, `#object`, `#ol`, `#optgroup`, `#option`, `#output`, `#p`, `#picture`, `#pre`, `#progress`, `#q`, `#rp`, `#rt`, `#ruby`, `#s`, `#samp`, `#script`, `#section`, `#select_tag`, `#slot`, `#small`, `#span`, `#strong`, `#style`, `#sub`, `#summary`, `#sup`, `#table`, `#tbody`, `#td`, `#template`, `#textarea`, `#tfoot`, `#th`, `#thead`, `#time`, `#title`, `#tr`, `#u`, `#ul`, `#var`, `#video`

- **Empty and void elements**: `#area`, `#base`, `#br`, `#col`, `#embed`, `#hr`, `#iframe`, `#img`, `#input`, `#link`, `#meta`, `#portal`, `#source`, `#track`, `#wbr`

All standard elements accept content, but empty and void elements do not.
However, you can still pass attributes to these elements.

```crystal
class Example
  include Blueprint::HTML

  private def blueprint
    iframe src: "page.html"

    meta charset: "utf-8"
  end
end

Example.new.to_s
```

Output:
```html
<iframe src="page.html"></iframe>

<meta charset="utf-8">
```

## Custom elements

If you need to use a custom element, you can use the `#element` instance method.

```crystal
class Example
  include Blueprint::HTML

  private def blueprint
    element("v-btn", to: "home") { "Home" }

    element("QCard", id: "welcome-card") { "Hello World!" }
  end
end

Example.new.to_s
```

Output:
```html
<v-btn to="home">Home</v-btn>

<QCard id="welcome-card">Hello World!<QCard>
```

If you frequently use a custom element, you can register it with the
`register_element` macro for easier access.

```crystal
class Example
  include Blueprint::HTML

  register_element :v_btn
  register_element :q_card, "QCard"

  private def blueprint
    v_btn(to: "home") { "Home" }

    q_card(id: "welcome-card") { "Hello World!" }
  end
end

Example.new.to_s
```

Output:
```html
<v-btn to="home">Home</v-btn>

<QCard id="welcome-card">Hello World!<QCard>
```

It is also possible to use `register_void_element` and `register_empty_element`
in case if you want elements without closing tags or elements that don't
accept content.
