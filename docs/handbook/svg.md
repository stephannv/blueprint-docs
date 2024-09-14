# SVG

Blueprint supports building SVG tags.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    svg xmlns: "http://www.w3.org/2000/svg", width: 30, height: 10 do
      g fill: :red do
        rect x: 0, y: 0, width: 10, height: 10
        rect x: 20, y: 0, width: 10, height: 10
      end
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="30" height="10">
  <g fill="red">
    <rect x="0" y="0" width="10" height="10"></rect>
    <rect x="20" y="0" width="10" height="10"></rect>
  </g>
</svg>
```

You should note that SVG tags methods are accessible only inside `#svg`
method block, using a SVG tag outside SVG block will result in a compilation
error.

```crystal
class ExamplePage
  include Blueprint::HTML

  private def blueprint
    svg do
      path d: "M3.75 9h16.5m-16.5 6.75h16.5" # OK
    end

    # Error: undefined local variable or method 'path' for ExamplePage
    path d: "M3.75 9h16.5m-16.5 6.75h16.5"
  end
end
```

The available SVG tags are: `a`, `animate`, `animateMotion`, `animateTransform`, `circle`, `clipPath`, `defs`, `desc`, `discard`, `ellipse`, `feBlend`, `feColorMatrix`, `feComponentTransfer`, `feComposite`, `feConvolveMatrix`, `feDiffuseLighting`, `feDisplacementMap`, `feDistantLight`, `feDropShadow`, `feFlood`, `feFuncA`, `feFuncB`, `feFuncG`, `feFuncR`, `feGaussianBlur`, `feImage`, `feMerge`, `feMergeNode`, `feMorphology`, `feOffset`, `fePointLight`, `feSpecularLighting`, `feSpotLight`, `feTile`, `feTurbulence`, `filter`, `foreignObject`, `g`, `image`, `line`, `linearGradient`, `marker`, `mask`, `metadata`, `mpath`, `path`, `pattern`, `polygon`, `polyline`, `radialGradient`, `rect`, `script`, `set`, `stop`, `style`, `svg`, `switch`, `symbol`, `text`, `textPath`, `title`, `tspan`, `use`, `view`.
