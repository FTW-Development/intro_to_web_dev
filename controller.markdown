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
