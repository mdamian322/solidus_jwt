SolidusJwt
==========

This gem gives [Solidus](https://github.com/solidusio/solidus) stores the ability to authenticate API requests with
JSON Web Tokens.

Installation
------------

Add solidus_jwt to your Gemfile:

```ruby
gem 'solidus'
gem 'solidus_jwt'
```

Bundle your dependencies and run the installation generator:

```shell
bundle
bundle exec rails g solidus_jwt:install
```

Configuration
-------------
TODO

Usage
-------------
### Generating and decoding a token:

```ruby
SolidusJwt::Config.configure do |config|
  config.jwt_secret = 'secret'
end

user = Spree::User.new email: 'email@example.com', id: 1
token = user.generate_jwt_token(expires_in: 1.hour.to_i) # Expiration is time in seconds
# eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NDA1MzIzNjcsImlhdCI6IjIwMTgtMTAtMjYgMDQ6Mzk6MjcgVVRDIiwiaWQiOjEsImVtYWlsIjoiZW1haWxAZXhhbXBsZS5jb20ifQ.LWqf_cfsMwB995AqN9wj5IseJqEZYaIHHIhf8Ej7WIc

SolidusJwt.decode(token)
# [{"exp"=>1540532367, "iat"=>"2018-10-26 04:39:27 UTC", "id"=>1, "email"=>"email@example.com"}, {"alg"=>"HS256"}]
```

### Distributing a token using solidus_auth_devise:

To have the `solidus_auth_devise` gem distribute a token back to the client
you can do the following:
```ruby
# app/controllers/application_controller.rb
include SolidusJwt::Distributor::Devise
```

When a user logs in, the redirect will contain the header `X-SPREE-TOKEN`.

Testing
-------

First bundle your dependencies, then run `rake`. `rake` will default to building the dummy app if it does not exist, then it will run specs, and [Rubocop](https://github.com/bbatsov/rubocop) static code analysis. The dummy app can be regenerated by using `rake test_app`.

```shell
bundle
bundle exec rake
```

When testing your applications integration with this extension you may use it's factories.
Simply add this require statement to your spec_helper:

```ruby
require 'solidus_jwt/factories'
```

Copyright (c) 2018 [name of extension creator], released under the New BSD License
