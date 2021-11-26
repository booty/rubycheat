```ruby
# app/helpers/application_helper.rb
module ApplicationHelper
	def full_title(page_title = "")
		base_title = "My Awesome Site"
		if page_title.empty?
			base_title
		else
			"#{page_title} | #{base_title}"
		end
	end
end
```

```erb
<!DOCTYPE html>
<html>
  <head>
    <title><%= full_title(yield(:title)) %></title>
  </head>
</html>
```

