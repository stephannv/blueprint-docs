# Conditional Rendering

Blueprints objects can implement a `#render?` method, if this method returns
false, the blueprint will not be rendered. This allows you extract the
conditional logic from component consumer and put in the component itself.

```crystal
class DraftAlertComponent
  include Blueprint::HTML

  def initialize(@article : Article); end

  private def blueprint
    div(class: "alert alert-warning") { "This is a draft" }
  end

  private def render?
    @article.draft?
  end
end

class ArticlePage
  include Blueprint::HTML

  def initialize(@article: Article); end

  private def blueprint
    # Instead of writing:
    # if @article.draft?
    #   render DraftAlert.new(@article)
    # end
    render DraftAlertComponent.new(@article)

    h1 { @article.title }
  end
end

article = Article.new(title: "Hello Blueprint", draft: false)

# DraftAlert will not be rendered
ArticlePage.new(article: article).to_s
```

Output:

```html
<h1>Hello Blueprint</h1>
```
