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
 		
when /coders/new is requested, just like before, Rails automatically looks for new.html.erb:

	<h1>New coder</h1>

	<%= render 'form' %>

	<%= link_to 'Back', coders_path %>

`render 'form'` tells rails to look for _form.html.erb
  
