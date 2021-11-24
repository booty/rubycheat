# Rendering

```ruby
# application/controllers/xxx_controller.rb
def FooController
	def hello
		# tries to render application/hello
		render

		# Rendering, rawdog style like a savage. by default, does not use layout
		render html: "hello, world!"

		# Rendering an action
		render action: "foo"    # looks for #foo, renders that action
		render action: "foo", layout: false
	  render action: "foo", layout: "some custom layout"

		# Rendering a partial - most commonly used for AJAX calls
	  # that only update a particular element. By default, layout
	  # not used.
	 	render partial: "person", locals: {a: 123, b: "dog"}
		render partial: "person", object: @new_person  # ???
		render partial: "person", collection: @winners  # ???
		render partial: "person", collection: @winners, as: :person
		render partial: "shared/note", collection: @new_notes  # render partial outside current controller
		render partial: "broken", status: 500

		# Rendering a template
		# Renders the template located in [TEMPLATE_ROOT]/weblog/show.r(html|xml)
	  # (in Rails, app/views/weblog/show.erb)
	  render :template => "weblog/show"
	  render :template => "weblog/show", :locals => {:customer => Customer.new}

		# TODO: streaming?
	end
end
```

advfkvdflsv

