# Controllers
The scaffold generater we previously ran also created a controller and views that allow for creating, editing and viewing coders.

We also configured config/routes.rb to load the index action from the coders controller with `root :to => 'coders#index'.

So now when we go to http://localhost:3000, the following happens:

1. The browser issues a request for the / URL, that is, the root url.
2. Rails routes / to the index action in the Coders controller(because we told it to)
3. The index action asks the Coder model to retrieve all coders (Coder.all).
4. The Coder model pulls all the coders from the database.
5. The Coder model returns the list of coders to the controller.
6. The controller captures the coders in the @coders variable, which is passed to the index view.
7. The view uses embedded Ruby to render the page as HTML.
8. The controller passes the HTML back to the browser.

Let's look at the code that makes this happen.

# Methods and Actions

A controller is a ruby class that inherits from Application controller. During #2 from last slide, an instance of the controller class is created as defined by config/routs.rb. So the line

	root :to => 'coders#index'

we added to `config/routes.rb` will create and instance of the class below and run the index method.
	
	class CodersController < ApplicationController
		# GET /coders
  		# GET /coders.json
  		def index
    		@coders = Coder.all
 		end

 		# GET /coders/1
 		# GET /coders/1.json
  		def show
  		end

  		# GET /coders/new
  		def new
    		@coder = Coder.new
 		end
 
 		...
 		
  	end

Any variable's defined in the controller, in this case `@coders` can then be accessed in the accompanying view that we'll see in the next slide.

# Controller with Views
	
In accordance with the "convention over configuration" principle, Rails will automatically look for the `action_name.html.erb` template and render it. In our case, this would be `app/views/coders/index.html.erb`.

Let's look at that file and see how the `@coder` variable and accompanying, inherited methods that come from the controller.

So that's how data is passed to the view, let's look at how it's passed back.

# Form Submission

Going back to the controller, let's look at the `new` method.
 
  		# GET /coders/new
  		def new
    		@coder = Coder.new
 		end
 		
when `/coders/new` is requested, just like before, Rails automatically looks for new.html.erb:

	<h1>New coder</h1>

	<%= render 'form' %>

	<%= link_to 'Back', coders_path %>

`render 'form'` tells rails to look for _form.html.erb. `render` can also be used in controllers to explicitly specify which view to render. An example:

	def update
  		@coders = Coder.find(params[:id])
  		if @coder.update(params[:coder])
    		redirect_to(@coder)
  		else
    		render "edit"
  		end
	end
 
This will render the `views/coders/edit.html.erb` file. 

Let's look at the _form.html.erb.

# Accessing Post Data
When the form is submitted, the `create` method in the controller is ran and the data is passed via a post object, which can be accessed using `params` as shown in this private method in the controller.

	def coder_params
      	params.require(:coder).permit(:first, :last, :age, :features_completed, :commits_per_season, :pull_requests)
    end
    
    # POST /coders
    # POST /coders.json
    def create
    	@coder = Coder.new(coder_params)

    	respond_to do |format|
      		if @coder.save
        		format.html { redirect_to @coder, notice: 'Coder was successfully created.' }
        		format.json { render action: 'show', status: :created, location: @coder }
      		else
        		format.html { render action: 'new' }
        		format.json { render json: @coder.errors, status: :unprocessable_entity }
      		end
    	end
 	end
    
If it can properly save the coder data, then the `redirect_to` method, which redirects to any URL, redirects to that coder that was just create, which runs the show method, and of course then renders the show.html.erb.

	# GET /coders/1
  	# GET /coders/1.json
  	def show
  	end