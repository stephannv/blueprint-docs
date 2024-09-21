# Forms

## Form Builder

Using `#form_builder` you can access some utility methods to build labels and inputs:

```crystal
class ExamplePage
  include Blueprint::HTML

  def blueprint
    form_builder action: "/sign-in", method: :post do |form|
      form.label :email
      form.email_input :email

      form.label :password
      form.password_input :password
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<form action="/sign-in" method="post">
  <label for="email">Email</label>
  <input type="email" id="email" name="email">

  <label for="password">Password</label>
  <input type="password" id="password" name="password">
</form>
```

When passing a scope to the form builder, it will be used to build input IDs and
names:

```crystal
class ExamplePage
  include Blueprint::HTML

  def blueprint
    form_builder scope: :user, action: "/sign-in", method: :post do |form|
      form.label :email
      form.email_input :email

      form.label :password
      form.password_input :password
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<form action="/sign-in" method="post">
  <label for="user_email">Email</label>
  <input type="email" id="user_email" name="user[email]">

  <label for="user_password">Password</label>
  <input type="password" id="user_password" name="user[password]">
</form>
```

## Inputs

If you don't need a form builder and still want to build inputs easily, you can
use the `#inputs` method, it is similar to form builder but it doesn't create a
form tag around the inputs. You can provide a scope or even use it inside the
form builder.

```crystal
class ExamplePage
  include Blueprint::HTML

  def blueprint
    inputs do |builder|
      builder.label :query
      builder.text_input :query
    end

    inputs :search do |builder|
      builder.label :query
      builder.text_input :query
    end

    form_builder scope: :user do |form|
      form.label :username
      form.text_input :username

      inputs :settings do |settings|
        settings.label :subdomain
        settings.text_input :subdomain
      end
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<label for="query">Query</label>
<input type="text" id="query" name="query">

<label for="search_query">Query</label>
<input type="text" id="search_query" name="search[query]">

<form>
  <label for="user_username">Username</label>
  <input type="text" id="user_username" name="user[username]">

  <label for="settings_subdomain">Subdomain</label>
  <input type="text" id="settings_subdomain" name="settings[subdomain]">
</form>
```

## Custom Form Builder

You can create your own form builder by subclassing `Blueprint::Form::Builder(T)`
and use `builder: YourFormBuilder` in the `#form_builder` method.

```crystal
class MyFormBuilder(T) < Blueprint::Form::Builder(T)
  def text_input(attribute : Symbol, **html_options)
    super(attribute, **html_options.merge(class: "form-input"))
  end

  def money_input(attribute : Symbol, **html_options)
    text_input(attribute, **html_options.merge(inputmode: :numeric, mask: "$#,##"))
  end

  def input_wrapper(&)
    div class: "form-control" do
      yield
    end
  end
end

class ExamplePage
  include Blueprint::HTML

  def blueprint
    form_builder scope: :income, builder: MyFormBuilder do |form|
      form.input_wrapper do
        form.label :description
        form.text_input :description
      end

      form.input_wrapper do
        form.label :amount
        form.money_input :amount
      end
    end
  end
end

ExamplePage.new.to_s
```

Output:

```html
<form>
  <div class="form-control">
    <label for="income_description">Description</label>
    <input type="text" id="income_description" name="income[description]" class="form-input">
  </div>

  <div class="form-control">
    <label for="income_amount">Amount</label>
    <input type="text" id="income_amount" name="income[amount]" inputmode="numeric" mask="$#,##" class="form-input">
  </div>
</form>
```

If you want to set your form builder as the default form builder, you can override
`default_form_builder` method to return your form builder class:

```crystal
class BasePage
  include Blueprint::HTML

  def default_form_builder
    MyFormBuilder
  end
end
```

## Methods

These are the available methods for form/input builder:

#### `label`

```crystal
form.label :name
# <label for="name">Name</label>

form.label :name, "Custom label"
# <label for="name">Custom label</label>

form.label :name, class: "form-label" do
  "Nome"
end
# <label for="name" class="form-label">Nome</label>

form.label :name do
  span "Name"
  span(class: "label-addon") { "*required" }
end
# <label for="name">
#   <span>Name</span>
#   <span class="label-addon">*required</span>
# </label>

form.label :color, "Red", value: :red
# <label for="color_red">Red</label>
```

#### `checkbox_input`
The HTML specification says unchecked checkboxes are not successful, and thus
web browsers do not send them. To prevent this the `checkbox_input` generates an auxiliary
hidden field before every checkbox. The hidden field has the same name and its
attributes mimic an unchecked checkbox.

```crystal
form.checkbox_input :accepted
# <input type="hidden" name="accepted" value="0">
# <input type="checkbox" id="accepted" name="accepted" value="1">
```

The default values are `"1"` for checked, and `"0"` for unchecked. You can provide
custom values if needed:
```crystal
form.checkbox_input :accepted, "yes", "no"
# <input type="hidden" name="accepted" value="no">
# <input type="checkbox" id="accepted" name="accepted" value="yes">
```

If you don't want the hidden field, you can pass `unchecked_value: nil`:

```crystal
form.checkbox_input :accepted, unchecked_value: nil
# <input type="checkbox" id="accepted" name="accepted" value="1">
```

#### `color_input`

```crystal
form.color_input :bg_color, class: "color-picker"
# <input type="color" id="bg_color" name="bg_color" class="color-picker">
```

#### `date_input`
```crystal
form.date_input :due_date, class: "date-picker"
# <input type="date" id="due_date" name="due_date" class="date-picker">
```

#### `datetime_input`
```crystal
form.datetime_input :due_date, class: "date-picker"
# <input type="datetime" id="due_date" name="due_date" class="date-picker">
```

#### `datetime_local_input`
```crystal
form.datetime_local_input :due_date, class: "date-picker"
# <input type="datetime-local" id="due_date" name="due_date" class="date-picker">
```

#### `email_input`
```crystal
form.email_input :email, class: "form-input"
# <input type="email" id="email" name="email" class="form-input">
```

#### `file_input`
```crystal
form.file_input :upload, class: "file-upload"
# <input type="file" id="upload" name="upload" class="file-upload">
```
#### `hidden_input`
```crystal
form.hidden_input :token, value: "1234"
# <input type="hidden" id="token" name="token" value="1234">
```

#### `month_input`
```crystal
form.month_input :date_month, class: "month-picker"
# <input type="month" id="date_month" name="date_month" value="month-picker">
```

#### `number_input`
```crystal
form.number_input :age, value: 18
# <input type="number" id="age" name="age" value="18">
```

#### `password_input`
```crystal
form.password_input :secret, class: "form-input"
# <input type="password" id="secret" name="secret" class="form-input">
```

#### `radio_input`
```crystal
form.radio_input :color, :red, class: "radio-input"
# <input type="radio" id="color_red" name="color" value="red" class="radio-input">
```

#### `range_input`
```crystal
form.range_input :volume, 1..99, step: 5
# <input type="range" id="volume" name="volume" min="1" max="99" step="5">

form.range_input :volume_db, min: -20, max: 20, step: 2
# <input type="range" id="volume_db" name="volume_db" min="-20" max="20" step="2">
```

#### `search_input`
```crystal
form.search_input :query, class: "form-input"
# <input type="search" id="query" name="query" class="form-input">
```

#### `tel_input`
```crystal
form.tel_input :phone, class: "form-input"
# <input type="tel" id="phone" name="phone" class="form-input">
```

#### `text_input`
```crystal
form.text_input :username, class: "form-input"
# <input type="text" id="username" name="username" class="form-input">
```

#### `time_input`
```crystal
form.time_input :paid_at, class: "time-picker"
# <input type="time" id="paid_at" name="paid_at" class="time-picker">
```

#### `url_input`
```crystal
form.url_input :base_url, class: "form-input"
# <input type="url" id="base_url" name="base_url" class="form-input">
```

#### `week_input`
```crystal
form.week_input :born_on, class: "form-input"
# <input type="week" id="born_on" name="born_on" class="form-input">
```

#### `submit`
```crystal
form.submit
# <input type="submit" value="Submit">

form.submit "Send"
# <input type="submit" value="Send">

form.submit "Upload", class: "btn btn-lg"
# <input type="submit" value="Upload" class="btn btn-lg">
```

#### `reset`
```crystal
form.reset
# <input type="reset" value="Reset">

form.reset "Clear"
# <input type="reset" value="Clear">

form.reset "Clear", class: "btn btn-lg"
# <input type="reset" value="Clear" class="btn btn-lg">
```
