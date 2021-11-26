[https://guides.rubyonrails.org/routing.html](https://guides.rubyonrails.org/routing.html#segment-constraints)

TODO: Test from w/in Rails console

```bash
rails routes
rails routes --expanded

# grep option (better formatting than piping through grep or ag?)
rails routes -g admin

# only routes from a specific controller
rails routes -c users
rails routes -c admin/users
rails routes -c Comments
rails routes -c Articles::CommentsController
```

