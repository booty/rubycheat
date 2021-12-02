[Rails Guides: Form Helpers](https://guides.rubyonrails.org/form_helpers.html)

# Binding Basics

In your controller, or elsewhere:

  ```ruby
  @article = Article.find(42)
  ```

In view:

  ```erb
  <%= form_with model: @article do |form| %>
    <%= form.text_field :title %>
    <%= form.text_area :body, size: "60x10" %>
    <%= form.submit %>
  <% end %>
  ```

Produces:

  ```html
  <form action="/articles/42" method="post" accept-charset="UTF-8" >
    <input name="authenticity_token" type="hidden" value="..." />
    <input type="text" name="article[title]" id="article_title" value="My Title" />
    <textarea name="article[body]" id="article_body" cols="60" rows="10">
      My Body
    </textarea>
    <input type="submit" name="commit" value="Update Article" data-disable-with="Update Article">
  </form>
  ```

# Non-Model Attributes

You can include any attributes you want, and access via

  ```ruby
  params[:article][:my_nifty_non_attribute_input]
  ```

# Embedding with Fields_For

  ```erb
  <%= form_with model: @person do |person_form| %>
    <%= person_form.text_field :name %>
    <%= fields_for :contact_detail, @person.contact_detail do |contact_detail_form| %>
      <%= contact_detail_form.text_field :phone_number %>
    <% end %>
  <% end %>
  ```

  ```html
  <form action="/people" accept-charset="UTF-8" method="post">
    <input type="hidden" name="authenticity_token" value="bL13x72pldyDD8bgtkjKQakJCpd4A8JdXGbfksxBDHdf1uC0kCMqe2tvVdUYfidJt0fj3ihC4NxiVHv8GVYxJA==" />
    <input type="text" name="person[name]" id="person_name" />
    <input type="text" name="contact_detail[phone_number]" id="contact_detail_phone_number" />
  </form>
  ```

# HAPPY PATH: Use Resource-Based Routing

  ```ruby
  # Creating a new article
  form_with(model: @article, url: articles_path)   # BOO!
  form_with(model: @article)                       # YAY!

  # Editing an existing article
  form_with(model: @article, url: article_path(@article), method: "patch")   # BOO!
  form_with(model: @article)                                                 # YAY! same as creating!
  ```

# Namespaces

  ```ruby
  # assume we have an "admin" namespace
  form_with model: [:admin, @article]

  # assume we have nested namespaces "admin" and "management"
  form_with model: [:admin, :management, @article]
  ```
















