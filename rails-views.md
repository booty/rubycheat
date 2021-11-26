# Yielding

  ```erb
  <% provide(:title, "Home") %>
  <!DOCTYPE html>
  <html>
  	<head>
  		<title><%= yield(:title) %></title>
  	</head>
  	<body>
  	</body>
  </html>
  ```

# Built-In Rails Helpers

TODO: Expand/complete

  ```erb
  <!-- <a href="/static_pages/about">About</a> -->
  <%= link_to "About", about_path %>
  ```