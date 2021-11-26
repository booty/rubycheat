
- [https://guides.rubyonrails.org/routing.html#segment-constraints](https://guides.rubyonrails.org/routing.html#segment-constraints)


# Resourceful Routes

In general, use these.

  ```ruby
  # this is the Rails "default"
  # GET         /photos           photos#index    display a list of all photos
  # GET         /photos/new       photos#new      return an HTML form for creating a new photo
  # POST        /photos           photos#create   create a new photo
  # GET         /photos/:id       photos#show     display a specific photo
  # GET         /photos/:id/edit  photos#edit     return an HTML form for editing a photo
  # PATCH/PUT   /photos/:id       photos#update   update a specific photo
  # DELETE      /photos/:id       photos#destroy  delete a specific photo
  resources :photos

  # can also do multiple
  resources :photos, :books, :videos

  # if you only need certain actions
  resources :photos, only: [:index, :new, :create]
  resources :photos, except: [:destroy]

  # useful if e.g. linking to a page that doesn't rely on id;
  # like if users#show always shows the current user
  get "profile", action: :show, controller: "users"
  get "profile", to: "users#show"
  ```

# Non-Resourceful Routes

In general, prefer resourceful routes.

  ```ruby
  # Manually create a route
  root "application#hello"

  # Some static pages
  get  "/help",    to: "static_pages#help"
  get  "/about",   to: "static_pages#about"
  get  "/contact", to: "static_pages#contact"

  # Optional bound parameter
  get "photos(/:id)", to: "photos#display"

  # Dynamic segments
  # note: params will also include querystring params
  # /photos/1?user_id=2 --> { controller: "photos", action: "show", id: "1", user_id: "2" }
  get "photos/:id", to: "photos#show"

  # Mix of dynamic and statis segments
  # /photos/1/with_user/2 --> { controller: "photos", action: "show", id: "1", user_id: "2" }
  get "photos/:id/with_user/:user_id", to: "photos#show"

  # Constraints
  get "photos/:id", to: "photos#show", constraints: { id: /[A-Z]\d{5}/ }
  get "photos/:id", to: "photos#show", id: /[A-Z]\d{5}/  # same as above, but more concise

  # Request-based constrains
  # works with any method on the Request object
  get "photos", to: "photos#index", constraints: { subdomain: "admin" }  # Request.subdomain must == 'admin'

  ```

# Naming Routes

  ```ruby
  # Will create logout_path and logout_url
  # logout_path => /exit
  get "exit", to: "sessions#destroy", as: :logout
  ```

# HTTP Verb Constraints

  ```ruby
  match "photos", to: "photos#show", via: [:get, :post]
  match "photos", to: "photos#show", via: :all
  ```

# Nested resources

Suppose you have:

  ```ruby
  class Magazine < ApplicationRecord
    has_many :ads
  end

  class Ad < ApplicationRecord
    belongs_to :magazine
  end
  ```

Your routes might look like:

  ```ruby
  # GET         /magazines/:magazine_id/ads           ads#index
  # GET         /magazines/:magazine_id/ads/new       ads#new
  # POST        /magazines/:magazine_id/ads           ads#create
  # GET         /magazines/:magazine_id/ads/:id       ads#show
  # GET         /magazines/:magazine_id/ads/:id/edit  ads#edit
  # PATCH/PUT   /magazines/:magazine_id/ads/:id       ads#update
  # DELETE      /magazines/:magazine_id/ads/:id       ads#destroy

  # Will also create helpers like magazine_ads_url and edit_magazine_ad_path
  # These helpers take an instance of Magazine as the first parameter (magazine_ads_url(@magazine))
  Rails.application.routes.draw do
    resources :magazines do
      resources :ads
    end
  end
  ```

# Shallow Nesting

  ```ruby
  resources :articles do
    resources :comments, shallow: true
  end
  ```

Same as...

  ```ruby
  resources :articles do
    resources :comments, only: [:index, :new, :create]
  end
  resources :comments, only: [:show, :edit, :update, :destroy]
  ```

# Namespaced Resources

One example use would be an "admin" namespace.

TODO: More examples of when this would be good/useful?

  ```ruby
  Rails.application.routes.draw do
    # Example: group controllers under an `Admin::` namespace
    # controllers would go in app/controllers/admin
    # GET         /admin/articles           admin/articles#index    admin_articles_path
    # GET         /admin/articles/new       admin/articles#new      new_admin_article_path
    # POST        /admin/articles           admin/articles#create   admin_articles_path
    # GET         /admin/articles/:id       admin/articles#show     admin_article_path(:id)
    # GET         /admin/articles/:id/edit  admin/articles#edit     edit_admin_article_path(:id)
    # PATCH/PUT   /admin/articles/:id       admin/articles#update   admin_article_path(:id)
    # DELETE      /admin/articles/:id       admin/articles#destroy  admin_article_path(:id)
    namespace :admin do
      resources :articles, :comments
    end
  end
  ```

Can use `root` too:

  ```ruby
  namespace :admin do
    root to: "admin#index"
  end
  ```

# Advanced Constraints

Can provide an object that responses to `#matches?` that Rails should use.

  ```ruby
  class RestrictedListConstraint
    def initialize
      @ips = RestrictedList.retrieve_ips
    end

    def matches?(request)
      @ips.include?(request.remote_ip)
    end
  end

  Rails.application.routes.draw do
    get "*path", to: "restricted_list#index",
      constraints: RestrictedListConstraint.new
  end
  ```

Can supply as lambda...

  ```ruby
  Rails.application.routes.draw do
    get
      "*path",
      to: "restricted_list#index",
      constraints: lambda { |request| RestrictedList.retrieve_ips.include?(request.remote_ip) }
  end
  ```

Or block form. (Useful when applying same rule to several routes)

  ```ruby
  Rails.application.routes.draw do
    constraints(RestrictedListConstraint.new) do
      get "*path", to: "restricted_list#index"
      get "*other-path", to: "other_restricted_list#index"
    end
  end
  ```

Can also use lambda in block form. (Example omitted)

# Route Globbing / Wildcard Segments

  ```ruby
  # photos/12              => params[:other] = "12"
  # photos/long/path/to/12 => params[:other] = "long/path/to/12"
  get "photos/*other", to: "photos#unknown"

  # zoo/woo/foo/bar/baz    => params is {a: "zoo/woo", b: "bar/baz"}
  get "*a/foo/*b", to: "test#index"
  ```

# Redirection

  ```ruby
  get "/stories", to: redirect("/articles")
  get "/stories/:name", to: redirect("/articles/%{name}")
  get "/stories/:name", to: redirect { |path_params, req| "/articles/#{path_params[:name].pluralize}" }
  get "/stories", to: redirect { |path_params, req| "/articles/#{req.subdomain}" }

  # Default is "301 Moved Permanently" -- can override if needed
  get "/stories/:name", to: redirect("/articles/%{name}", status: 302)
  ```

# Routing to Rack Applications

  ```ruby
  match "/application.js", to: MyRackApp, via: :all
  ```

# Multiple Files with Draw

  ```ruby
  Rails.application.routes.draw do
    get "foo", to: "foo#bar"

    draw(:admin) # Will load another route file located in `config/routes/admin.rb`
  end
  ```