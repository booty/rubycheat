# Basic Principles

- Rails will put a hidden `authenticity_token` field in each form
- For each form input, an HTML ID attribute is generated from its name
  - Useful for styling/JS
- Helpers are:



`data_disable_with`

# Basic Example

  ```erb
  <%= form.text_field, :search %>
  <%= form.label :pet_dog, "I own a dog" %>
  <%= form.radio_button :age, "child" %>
  <%= form.text_area :message, size: "70x5" %>
  <%= form.hidden_field :parent_id, value: "foo" %>
  <%= form.password_field :password %>
  <%= form.number_field :price, in: 1.0..20.0, step: 0.5 %>
  <%= form.range_field :discount, in: 1..100 %>
  <%= form.date_field :born_on  # uses HTML5 %>
  <%= form.date_select :born_on # does not use HTML45%>
  <%= form.time_field :started_at # HTML5 %>
  <%= form.time_select :started_at # no HTML5 %>
  <%= form.datetime_local_field :graduation_day # HTML5%>
  <%= form.datetime_local_select :graduation_day # no HTML5 %>
  <%= form.month_field :birthday_month %>
  <%= form.week_field :birthday_week %>
  <%= form.search_field :name %>
  <%= form.email_field :address %>
  <%= form.telephone_field :phone %>
  <%= form.url_field :homepage %>
  <%= form.color_field :favorite_color %>
  ```

  ```html
  <input type="text" id="query" name="query"/>
  <input type="checkbox" id="pet_dog" name="pet_dog" value="1" />
  <input type="radio" id="age_child" name="age" value="child" />
  <textarea name="message" id="message" cols="70" rows="5"></textarea>
  <input type="hidden" name="parent_id" id="parent_id" value="foo" />
  <input type="password" name="password" id="password" />
  <input type="number" name="price" id="price" step="0.5" min="1.0" max="20.0" />
  <input type="range" name="discount" id="discount" min="1" max="100" />
  <input type="date" name="born_on" id="born_on" />
  <input type="time" name="started_at" id="started_at" />
  <input type="datetime-local" name="graduation_day" id="graduation_day" />
  <input type="month" name="birthday_month" id="birthday_month" />
  <input type="week" name="birthday_week" id="birthday_week" />
  <input type="search" name="name" id="name" />
  <input type="email" name="address" id="address" />
  <input type="tel" name="phone" id="phone" />
  <input type="url" name="homepage" id="homepage" />
  <input type="color" name="favorite_color" id="favorite_color" value="#000000" />
  ```

# Slightly Less Basic Example

TODO: test

  ```html
  <%= form_with url: "/search", method: :get do |f| %>
    <p>
      <%= form.label :query, "Search For:" %>
      <%= form.text_field :query %>
    </p>

    <!-- checkboxes
      will produce:
      <input type="checkbox" id="pet_dog" name="pet_dog" value="1" />
      <label for="pet_dog">I own a dog</label>
      <input type="checkbox" id="pet_cat" name="pet_cat" value="1" />
      <label for="pet_cat">I own a cat</label>
    -->
    <p>
      <%= form.label :pet_dog, "I own a dog" %>
      <%= form.check_box :pet_dog %>
    </p>
    <p>
      <%= form.label :pet_cat, "I own a cat" %>
      <%= form.check_box :pet_cat %>
    </p>

    <!-- radio buttons
      will produce:
      <input type="radio" id="age_child" name="age" value="child" />
      <label for="age_child">I am younger than 21</label>
      <input type="radio" id="age_adult" name="age" value="adult" />
      <label for="age_adult">I am over 21</label>
    -->
    <p>
      <%= form.radio_button :age, "child" %>
      <%= form.label :age_child, "I'm under 21" %>
    </p>
    <p>
      <%= form.radio_button :age, "adult" %>
      <%= form.label :age_adult, "I'm 21 or older" %>
    </p>



    <p>
      <%= form.label :query, "Search For:" %>
      <%= form.text_field :query %>
    </p>
  <% end %>
  ```

# Select Boxes / Dropdowns

Basic:

  ```ruby
  form.select :city, ["Philadelphia", "Norristown", "King of Prussia"]
  form.select :city, [["Philadelphia", "PHL"], ["Chicago", "CHI"], ["Madrid", "MD"]]
  form.select :city, [["Philadelphia", "PHL"], ["Chicago", "CHI"]], selected: "PHL"
  ```

With model binding, we don't need to specify which is selected.

  ```erb
  <% @person = Person.new(city: "MD") %>

  <!-- Madrid will automagically be selected -->
  <%= form_with model: @person do |form| %>
    <%= form.select :city, [["Berlin", "BE"], ["Chicago", "CHI"], ["Madrid", "MD"]] %>
  <% end %>
  ```

Option groups:

  ```erb
  <%= form.select(:city,
          "Europe" => [ ["Berlin", "BE"], ["Madrid", "MD"] ],
          "North America" => [ ["Chicago", "CHI"] ],
        },
        selected: "CHI")
  %>
  ```

  ```html
  <select name="city" id="city">
    <optgroup label="Europe">
      <option value="BE">Berlin</option>
      <option value="MD">Madrid</option>
    </optgroup>
    <optgroup label="North America">
      <option value="CHI" selected="selected">Chicago</option>
    </optgroup>
  </select>
  ```

# Selects, Checkboxs, and Radio Buttons from Database

`:id` is the value method; `:name` is the text label method.

  ```erb
  <%= form.collection_select :city_id, City.order(:name), :id, :name %>
  <%= form.collection_check_boxes :city_id, City.order(:name), :id, :name %>
  <%= form.collection_radio_buttons :city_id, City.order(:name), :id, :name %>
  ```




