

[Link to tutorial.](https://www.learnenough.com/ruby-on-rails-6th-edition-tutorial/filling_in_the_layout#sec-custom_css)

- Bootstrap natively uses Less
- Rails asset pipeline uses Sass
- So, use `bootstrap-sass` gem

  ```ruby
  # Gemfile
  gem 'bootstrap-sass', '3.4.1'
  ```

# Universal Styling, Applying to All Pages

  ```css
  /* app/assets/stylesheets/custom.scss */
  @import "bootstrap-sprockets";
  @import "bootstrap";

  /* universal */
  body {
    padding-top: 60px;
  }
  ```

# Nested Styles

  ```sass
  /* nested styles */
  .center {
    text-align: center;
    h1 {
      margin-bottom: 10px;
    }
  }

  /* also nested styles */
  #logo {
    color: #fff;
    &:hover {
      color: #eee;
    }
  }
  ```
# Variables

  ```sass
  /* variables */
  $light-gray: #777;

  h2 {
  	color: $light-gray;
  }
  ```
