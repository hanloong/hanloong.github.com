---
layout: post
title: EmberCLI + Rails with OAuth2
category: posts
---

# Introduction

Most apps I work on need authentication.
Split App has become a popular choice for web app architecture.
Use the standard of OAuth2 over bespoke solutions.
This is a todo list with authentication.

## Frontend Libraries Used

 - EmberCLI
 - EmberJs
 - Ember Data
 - Ember Simple Auth
 - Ember Simple Auth OAuth2
 - Ember-CLI-Materialize

## Server Libraries

 - Rails 4.2
 - Clearance
 - JSONAPI-resources
 - Doorkeeper
 - Rack-Cors

# API Server

## Bootstrap the rails server
~~~bash
$ rails new todo-api
$ cd todo-api
~~~

## Setup a homepage
~~~bash
$ rails generate controller static home
~~~

~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  use_doorkeeper
  root 'static#home'
end
~~~

## Setup the additional gems
~~~ruby
## Gemfile
gem 'clearance'
gem 'doorkeeper'
gem 'rack-cors', require: 'rack/cors'
gem 'jsonapi-resources'
~~~

~~~bash
$ bundle install
$ rails generate clearance:install
$ rails generate doorkeeper:install
$ rake db:migrate
~~~
### Configure Clearance

~~~html
<!-- app/views/layouts/application.html.erb -->
<!DOCTYPE html>
<html>
<head>
  <title>Hisight</title>
  <%= stylesheet_link_tag    'application', media: 'all' %>
  <%= csrf_meta_tags %>
</head>
<body>

<% if signed_in? %>
  <%= current_user.email %>
  <%= button_to "Sign out", sign_out_path, method: :delete %>
<% else %>
  <%= link_to "Sign in", sign_in_path %>
<% end %>

<%= yield %>

</body>
</html>
~~~

~~~ruby
# app/controller/application_controller.rb
class ApplicationController < ActionController::Base
  include Clearance::Controller
  protect_from_forgery with: :exception
end
~~~

### Configure CORS

~~~ruby
# config/application.rb
module TodoApi
  class Application < Rails::Application
    config.middleware.insert_before 0, "Rack::Cors", :debug => true, :logger => (-> { Rails.logger }) do
      allow do
        origins '*'
        resource 'api/v1/*',
          headers: :any,
          methods: [:get, :post, :patch, :delete, :put, :options, :head],
          max_age: 0
      end
    end
  end
end
~~~

### Configure Doorkeeper

~~~ruby
# config/initializers/doorkeeper.rb
Doorkeeper.configure do
  orm :active_record

  resource_owner_authenticator do
    @user = env[:clearance].current_user

    unless @user
      session[:return_to] = request.fullpath
      redirect_to(sign_in_url)
    end

    @user
  end

  resource_owner_from_credentials do |routes|
    User.authenticate(params[:username], params[:password])
  end
end
~~~

## Create the Task model

~~~bash
$ rails generate model task title:string user:references done:boolean
$ rake db:migrate
~~~

## Setup the API

### Configure the routes
~~~ruby
# config/routes.rb
Rails.application.routes.draw do
  use_doorkeeper
  root 'static#home'

  namespace :api do
    namespace :v1  do
      jsonapi_resources :tasks
    end
  end
end
~~~

### Add the resource
~~~ruby
# app/resources/api/v1/task_resource.rb
class Api::V1::TaskResource < JSONAPI::Resource
  attributes :title, :complete
  has_one :user
end
~~~

### Add the controller
~~~bash
$ rails g controller api/v1/tasks --no-assets --no-helper
~~~

~~~ruby
# app/controllers/api/v1/tasks_controller.rb
class Api::V1::TasksController < JSONAPI::ResourceController
end
~~~

### Require Login for API

~~~ruby
# app/controllers/api/v1/tasks_controller.rb
class Api::V1::TasksController < JSONAPI::ResourceController
  before_action :doorkeeper_authorize!
end
~~~

### Allow TaskResource access to logged in user
~~~ruby
# app/controllers/api/v1/tasks_controller.rb
class Api::V1::TasksController < JSONAPI::ResourceController
  before_action :doorkeeper_authorize!

  def context
    {current_user: current_user}
  end

  def current_user
    @current_user ||= User.find(doorkeeper_token[:resource_owner_id])
  end
end
~~~

### Assign the new tasks to the logged in user
~~~ruby
# app/resources/api/v1/task_resource.rb
class Api::V1::TaskResource < JSONAPI::Resource
  # ....
  before_create :set_user

  # prevent the api call from manually assigning user
  def self.updatable_fields(context)
    super - [:user]
  end

  def self.creatable_fields(context)
    super - [:user]
  end

  private

  def set_user
    # grab the current_user from context hash
    current_user = context[:current_user]
    # assign user to the task model
    @model.assign_attributes({user: current_user})
  end
end
~~~

# Ember Frontend

## Bootstrap the ember project

~~~bash
$ ember new todo-js
$ cd todo-js
~~~

## Install the addons
~~~bash
$ ember install ember-cli-simple-auth
$ ember install ember-cli-simple-auth-oauth2
$ ember install ember-cli-materialize
~~~

## Setup css framework
~~~bash
$ ember g ember-cli-materialize
~~~

## Create the routes

## Configure Login
## Configure OAuth
## Configure Data Adapter
## Add the task model
## Create new tasks
## Show tasks
## Add completed Action
## Split out completed tasks
