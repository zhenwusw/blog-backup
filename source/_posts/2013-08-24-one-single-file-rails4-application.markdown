---
layout: post
title: "one-single-file-rails-application"
date: 2013-08-24 22:22
comments: true
categories:
---

Goodies:
  * if you find bugs inside rails, you can post as Gist with single file application without a bloated rails application
  *

```
require "sinatra::Base"
class MyApp < Sinatra::Base
  get "/" do
    "Hello World!"
  end
end
run MyApp.new
```
config.ru: rackup file => rackup application

rails rackup file:


```
# config.ru
require ::File.expand_path('../config/environment', __FILE__)
run MyApp::Application
```


```
# config/environemnt.rb
require File.expane_path('../application', __FILE__)
MyApp::Application.initialize!
```

```
# config/application.rb
require File.expan_path('../boot', __FILE__)
module MyApp
  class Application < Rails::Application
```

```
# config/boot.rb
ENV['BUNDLE_GEMFILE'] ||= File.expand_path('../../Gemfile', __FILE__)
require 'bundler/setup'
```

# rails process
* config/boot.rb # setup the load path
* config/application.rb # define the application
* config/environment.rb # initialize the application
* config.ru # setup rack


In new rails version, we have a Rakefile that just load `application` file
because we don't need to do the initialization process

```
# Rakefile
require File.expand_path('../config/application', __FILE__)
MyApp::Application.load_tasks
```

```
# lib/task/my_tasks.rake
task :hello => :environment do
  # ...
end
```

example
```
get "/", to: "posts#index"
-> get "/", to: PostsController.action(:index)
```

* match(get, post, put, delete) # match on the full path
* mount # match on the path prefix


Summary
# The Rack API
# PostsController.action(:index)
# match vs. mount

Middleware
----
3 mdiddleware stack
  * web middleware
  * rails middleware
  * controller middleware


Things we got for free

  * middleware
  * before/after action hooks
  * streaming
  * layout & render


Rendering stack
---

include ActionView::Context


Topics

* Single-file Rails applications
* Rack and the Rails router
* Middleware stack
* Rails rendering stack
