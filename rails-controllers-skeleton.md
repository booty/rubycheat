```ruby
class PostController < ApplicationController
	before_action :set_post, only: [:show, :edit, :update, :destroy]
	before_action :authenticate_user!  # if using Devise

	def index
		if session[:q].present?
			params[:page] = 1
			# lol, SQL injection?
			@posts = Post.where "title like ?", "%#{session[:q]}%"
		else
			@posts = Post.all
		end
	end

	def show
	end

	def new
		@post = Post.new
	end

	def edit
	end

	def create
		@post = Post.new(post_params)
		if @post.save
			redirect_to @post, notice: "Created successfully, bro!"
		else
			render :new
		end
	end

	def update
		if @post.update(post_params)
			redirect_to @post, notice: "Updated successfully, dude!"
		else
			render :edit
		end
	end

	def destroy
		@post.destroy
		redirect_to posts_url, notice: "You nuked that thing!"
	end

	private

	def set_post
		@post = Post.find(params[:id])
	end

	def post_params
		params.require(:post).permit(:title, :body, :image_url)
	end
end