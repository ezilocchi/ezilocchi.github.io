---
layout: post
title: "getting started rails 4 with mongodb"
date: 2013-04-26 09:49:31 -0300
comments: true
categories: Rails
---

Introduction
------------

The idea is to create a Ruby on Rails application  from scratch, that
connects with MongoDB. The gems involve are:

* **MongoID**:  To integrate Rails and MongoDB
* **Slim**: An alternative template engine to ERB (the Rails default one).
* **Rspec**: A BDD gem to express the tests as specifications
* **Cucumber**: A BBD gem to express the acceptance tests in Gherkin Syntax
NOTE:  You can find the source code for this tutorial here

Technology Stack: **MongoDB**, **Ruby**, **Rails**, **GIT (Optional)**, **VIM (Optional)**

<!-- more -->

Create a rails app without active record and test unit 
------------------------------------------------------

Since we are using MongoDB, a NoSQL database, we don’t need ActiveRecord, so we can use the option `--skip-active-record` in order to avoid the creation of all the files and configuration related.

In the same way, we want to use Rspec and Cucumber for automation testing, so we can omit the Unit Test installation as well with the option `--skip-test-unit`.

```
rails new myapp --skip-active-record --skip-test-unit
```

Installing MongoID gem
----------------------

Open the Gemfile and add the dependency `gem “mongoid”` and then run `bundle install`.

Finally run
```
rails generate mongoid:config
```

Now any time that you run a generate command that involves models creation it will invoke MongoID instead of ActiveRecord.

NOTE: At the time of writing this blog the lastest release for MongoID (3.0.5) is not compatible with Rails 4, so you have to use the master version instead:
```
+  gem 'mongoid', git: 'git://github.com/mongoid/mongoid.git'
```

Installing Slim gem
-------------------

Open the Gemfile and add the gem dependency: `gem “slim-rails”` and run `bundle install`.

Now any time that you run a generate command that involves template creation it will use Slim instead of ERB.

Installing Rspec gem
--------------------

One more time we add the dependency:

```
+  group :test do
+    gem "rspec-rails"
+  end
```
And then run:

```
rails generate rspec:install
```

Since we are using MongoID and not ActiveRecord, you need to comment these two lines from spec/spec_helper.rb:
```
# Remove this line if you're not using ActiveRecord or ActiveRecord
fixtures
-  config.fixture_path = "#{::Rails.root}/spec/fixtures"
+  # config.fixture_path = "#{::Rails.root}/spec/fixtures"
# If you're not using ActiveRecord, or you'd prefer not to run each of
your
# examples within a transaction, remove the following line or assign
false
# instead of true.
-  config.use_transactional_fixtures = true
+  # config.use_transactional_fixtures = true
```

Installing Cucumber gem
-----------------------

Again we add the dependency:
```
+  group :test do
+    gem 'cucumber-rails', :require => false
+    gem 'database_cleaner'
+  end
```

And the command to install it:
```
rails generate cucumber:install
```

Since we are using MongoID and not ActiveRecord, you need to change the database cleaner strategy from transaction to truncation in features/support/env.rb:

```
-  DatabaseCleaner.strategy = :transaction
+  DatabaseCleaner.strategy = :truncation
```

Let’s create some entities
--------------------------

To show the impact of these gems in a standar generate command, let see what has changed.

```
rails generate scaffold product name description price:float
```
```
invoke  mongoid
create    app/models/product.rb
invoke  resource_route
route    resources :products
invoke  jbuilder_scaffold_controller
create    app/controllers/products_controller.rb
invoke    slim
create      app/views/products
create      app/views/products/index.html.slim
create      app/views/products/edit.html.slim
create      app/views/products/show.html.slim
create      app/views/products/new.html.slim
create      app/views/products/_form.html.slim
invoke    helper
create      app/helpers/products_helper.rb
invoke    jbuilder
exist      app/views/products
create      app/views/products/index.json.jbuilder
create      app/views/products/show.json.jbuilder
invoke  assets
invoke    coffee
create      app/assets/javascripts/products.js.coffee
invoke    scss
create      app/assets/stylesheets/products.css.scss
invoke  scss
create    app/assets/stylesheets/scaffolds.css.scss
```

This is the command output, let’s see what has changed from the default one:

* **views**: We are invoking slim so we are generating slim extension instead of erb.
* **db**: We are not generating migrations. Since MongoDB is a schemaless database, we don’t need to create tables.
* **model**: We are invoking mongoid now, so we are creating models prepared to work with Mongo:
```
+  class Product
+    include Mongoid::Document
+    field :name, type: String
+    field :description, type: String
+    field :price, type: Float
+  end
```

Resources:
----------
* http://stackoverflow.com/questions/15512683/comparability-issue-rails-4-beta-ruby-2-0-0-mongoid
* http://github.com/ezilocchi/rails_with_mongo_example
