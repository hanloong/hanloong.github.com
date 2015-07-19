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

### Configure CORS

~~~ruby
# CORS for API
config.middleware.insert_before 0, "Rack::Cors", :debug => true, :logger => (-> { Rails.logger }) do
  allow do
    origins '*'
    resource '*',
      :headers => :any,
      :methods => [:get, :post, :patch, :delete, :put, :options, :head],
      :max_age => 0
  end
end
~~~

### Configure Doorkeeper

~~~ruby
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

# Ember Frontend

## Bootstrap the ember project

~~~bash
$ ember new todo-js
$ cd todo-js
~~~
