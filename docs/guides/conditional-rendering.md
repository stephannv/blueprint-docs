# Conditional Rendering

Blueprints objects can implement a `#render?` method, if this method returns
false, the blueprint will not be rendered. This allows you extract the
conditional logic from component consumer and put in the component itself.

=== ":simple-crystal: DraftAlert"

    ```crystal
    class DraftAlert
      include Blueprint::HTML

      def initialize(@article : Article); end

      private def blueprint
        div(class: "alert alert-warning") { "This is a draft" }
      end

      private def render?
        @article.draft?
      end
    end
    ```

=== ":simple-crystal: ArticlePage"

    ```crystal
    class ArticlePage
      include Blueprint::HTML

      def initialize(@article: Article); end

      private def blueprint
        # Instead of writing:
        # if @article.draft?
        #   render DraftAlert.new(@article)
        # end
        render DraftAlert.new(@article)

        h1 { @article.title }
      end
    end
    ```

=== ":simple-crystal: Main"

    ```crystal
    article = Article.new(title: "Hello Blueprint", draft: false)
    page = ArticlePage.new(article: article)
    page.to_html
    ```

=== ":simple-html5: Output"

    ```html
    <h1>Hello Blueprint</h1>
    ```
