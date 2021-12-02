
If using `form_with` without a model, must set multipart/form-data manually.

  ```erb
  <% form_with url: "/uploads", multipart: true do |form| %>
    <%= form.file_field :picture %>
  <% end %>
  ```

If using `form_with` with a model it's automagic. (TODO: Cool, but why?)

  ```erb
  <%= form_with model: @person do |form| %>
    <%= form.file_field :picture %>
  <% end %>
  ```

Saving the file:

  ```ruby
  def upload
    uploaded_file = params[:picture]
    File.open(Rails.root.join('public', 'uploads', uploaded_file.original_filename), 'wb') do |file|
      file.write(uploaded_file.read)
    end
  end
  ```

TODO: ActiveStorage is used to do the rest...
