# Intro to Web Development w/ Rails

We'll be doing a very cursory look at git, and a lay of the land of rails. 

Presentation by FTW Development @ftwdevelopment
Hunter Barrington @hbarrin

source found at [https://github.com/FTW-Development/intro_to_web_dev](https://github.com/FTW-Development/intro_to_web_dev)

This talk should last an hour

I assume you're on Mac (OS X) but linux/ubuntu works great too

# Tools and Setup

You'll need [git](https://help.github.com/articles/set-up-git) and [rvm](http://rvm.io/) to start. RVM is a "ruby version manager" which allows you to switch between language versions and project specific library (or gem) dependencies easily. Lastly we'll install rails 

Code:
* `brew install git`
* `\curl -sSL https://get.rvm.io | bash`
* `rvm install 2.0`
* `gem install rails`


Ref:
* [For Mac's this is a half decent link though a bit dated](http://www.moncefbelyamani.com/how-to-install-xcode-homebrew-git-rvm-ruby-on-mac/)
* You will need xcode and the command line tools installed on mac so you have GCC around
* On Ubuntu you can just use apt-get/aptitude to do dependency/package management


# Start a Project

We'll go ahead and use rail's internal tools to start a new project.

Code:
* `rails new fantasy_coding`

# Take a look at what came out

* Gemfile - list of gems (libraries) installed by default
* config - configuration files for the project
    * convention vs configuration (Rails takes a middle [or slightly left of middle] approach)
    * database, environments, gem/plugins
* app - application code
    * MVC
    * some other stuff (workers, mailers, assets, etc...)

Code:
* `rails s`
* http://localhost:3000

A quick note: There's help all over the place
Other things Rails cares about:
* DRY - Don't Repeat Yourself (OOP)
* Good API design
* Simple and elegant solutions

Things Rails isn't so great at
* Realtime applications/event-driven apps (check out nodejs)
* Having one source of truth about best practices, gems to use, or even documentation

# Initialize
* `touch .ruby-version < "ruby-2.0"`
* `touch .ruby-gemset < "fantasy-coding"`
* `git init`
* `git add -A`
* `git commit -m "initial commit"`
* `git remote add origin URLGOESHERE` - if you have a remote

# Super Brief Git vs SVN

sections 1.1 - 1.3
http://git-scm.com/book/en/Getting-Started-About-Version-Control

http://git-scm.com/book/en/Git-Basics

# Start Configuring our App
Let's add a bunch of gems to make development more fun and a bit easier. We'll also use mysql rather than sqlite (consider nosql solutions like MongoDB or CouchDB)

Code for `Gemfile`:
    gem 'mysql2'
    group :development do
      gem 'thin'
      gem 'growl'
      gem 'guard'
      gem 'guard-bundler'
      gem 'guard-livereload'
      gem 'guard-rails'
      gem 'guard-rspec'
      gem 'rack-livereload'
      gem 'capistrano'
      gem 'capistrano-unicorn'
      gem 'rvm-capistrano'
      gem 'rb-fsevent', :require => false
      gem 'rb-inotify', :require => false
      gem 'rb-fchange', :require => false
    end

    group :development, :test do
      gem 'rspec-rails'
      gem 'factory_girl_rails'
    end

    group :test do
      gem 'cucumber-rails', :require => false
      gem 'launchy'
      gem 'capybara'
      gem 'database_cleaner'
      gem 'email_spec'
    end

Code for `config/database.yml`:
    development:
      adapter: mysql2
      encoding: utf8
      database: timetracker_dev
      username: root
      password:
      pool: 5
      timeout: 5000

    # Warning: The database defined as "test" will be erased and
    # re-generated from your development database when you run "rake".
    # Do not set this db to the same as development or production.
    test:
      adapter: mysql2
      encoding: utf8
      database: timetracker_test
      username: root
      password:
      pool: 5
      timeout: 5000

    staging:
      adapter: mysql2
      encoding: utf8
      database: timetracker_prod
      username: root
      password: <%= begin IO.read("/home/rails/.myapp_db") rescue "" end %>
      pool: 5
      timeout: 5000

    production:
      adapter: mysql2
      encoding: utf8
      database: timetracker_prod
      username: root
      password: <%= begin IO.read("/home/rails/.myapp_db") rescue "" end %>
      pool: 5
      timeout: 5000


# Creat our first model and some base code
We'll use scaffolding to create the whole MVC structure for a new model and its associated functionality


