# Tags

## Overview

All non-deprecated HTML tags are available as instance methods on
Blueprint::HTML. You can provide content either as the first positional argument
or via a block. The tag methods accept named arguments, which are then converted
into HTML attributes.

```crystal
class ExamplePage
  def blueprint
    div "Hello!" # passing content as argument

    span { "Hello!" } # passing content via block

    h1 "Hello!", class: "heading" # passing HTML attributes

    h2 class: "heading" { "Hello!" } # passing HTML attributes
  end
end

ExamplePage.new.to_s
```

Output:
```html
<div>Hello!</div>

<span>Hello!</span>

<h1 class="heading">Hello!</h1>

<h2 class="heading">Hello!</h2>
```

## Standard tags

The avaiable tag methods are:

- **Standard elements**: `#a`, `#abbr`, `#address`, `#article`, `#aside`, `#audio`, `#b`, `#bdi`, `#bdo`, `#blockquote`, `#body`, `#button`, `#canvas`, `#caption`, `#cite`, `#code`, `#colgroup`, `#data`, `#datalist`, `#dd`, `#del`, `#details`, `#dfn`, `#dialog`, `#div`, `#dl`, `#dt`, `#em`, `#fieldset`, `#figcaption`, `#figure`, `#footer`, `#form`, `#h1`, `#h2`, `#h3`, `#h4`, `#h5`, `#h6`, `#head`, `#header`, `#hgroup`, `#html`, `#i`, `#ins`, `#kbd`, `#label`, `#legend`, `#li`, `#main`, `#map`, `#mark`, `#menu`, `#meter`, `#nav`, `#noscript`, `#object`, `#ol`, `#optgroup`, `#option`, `#output`, `#p`, `#picture`, `#pre`, `#progress`, `#q`, `#rp`, `#rt`, `#ruby`, `#s`, `#samp`, `#script`, `#section`, `#select_tag`, `#slot`, `#small`, `#span`, `#strong`, `#style`, `#sub`, `#summary`, `#sup`, `#table`, `#tbody`, `#td`, `#template`, `#textarea`, `#tfoot`, `#th`, `#thead`, `#time`, `#title`, `#tr`, `#u`, `#ul`, `#var`, `#video`

- **Empty and void elements**: `#area`, `#base`, `#br`, `#col`, `#embed`, `#hr`, `#iframe`, `#img`, `#input`, `#link`, `#meta`, `#portal`, `#source`, `#track`, `#wbr`

All standard elements accept content, but empty and void elements do not.
However, you can still pass attributes to these elements.

```crystal
class ExamplePage
  def blueprint
    iframe src: "page.html"

    meta charset: "utf-8"
  end
end

ExamplePage.new.to_s
```

Output:
```html
<iframe src="page.html"></iframe>

<meta charset="utf-8">
```

## Custom tags

If you need to use a custom tag, you can use the `#element` instance method.

```crystal
class ExamplePage
  def blueprint
    element("v-btn", "Home", to: "home")

    element("QCard", id: "welcome-card") { "Hello World!" }
  end
end

ExamplePage.new.to_s
```

Output:
```html
<v-btn to="home">Home</v-btn>

<QCard id="welcome-card">Hello World!<QCard>
```

If you frequently use a custom tag, you can register it with the
`register_element` macro for easier access.

```crystal
class ExamplePage
  register_element :v_btn
  register_element :q_card, "QCard"

  def blueprint
    v_btn("Home", to: "home")

    q_card(id: "welcome-card") { "Hello World!" }
  end
end

ExamplePage.new.to_s
```

Output:
```html
<v-btn to="home">Home</v-btn>

<QCard id="welcome-card">Hello World!<QCard>
```

It is also possible to use `register_void_element` and `register_empty_element`
in case if you want elements without closing tags or elements that don't
accept content.
