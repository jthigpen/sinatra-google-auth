# Sinatra::GoogleAuth

Drop-in authentication for Sinatra with Google Apps.

## Installation

Add this line to your application's Gemfile:

    gem 'sinatra-google-auth'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install sinatra-google-auth

## Usage

The gem exposes a single `authenticate` helper that protects the endpoint with
Google OpenID authentication.

### Classic-Style Apps

```ruby
require 'sinatra'
require 'sinatra/google-auth'

get '*' do
  authenticate
  'hello'
end
```


### Modular Apps

```ruby
require 'sinatra/base'
require 'sinatra/google-auth'

class App < Sinatra::Base
  register Sinatra::GoogleAuth

  get '*' do
    authenticate
    'hello'
  end
end
```

## Doing something with the User

Define an `on_user` method in your app to do something with the user info after authentication.

```ruby
class App < Sinatra::Base
  register Sinatra::GoogleAuth

  def on_user(info)
    puts info.inspect
  end

  get '*' do
    authenticate
    'hello'
  end
end
```

## Configuration 

### Google Endpoint

Configure your Google Apps endpoint via setting the ENV var `GOOGLE_AUTH_URL`

    # if your Google Apps email address is me@mycompany.com then your auth URL is probably mycompany.com
    $ export GOOGLE_AUTH_URL=mycompany.com

or before requiring 

```ruby
ENV['GOOGLE_AUTH_URL'] = 'mycompany.com'

require 'sinatra'
require 'sinatra/google-auth'
```

### Session Secret 

Configure your session secret by setting `SESSION_SECRET` or `SECURE_KEY` ENV vars.


    $ export SESSION_SECRET='super secure secret'

The 'SecureKey' add-on sets the `SECURE_KEY` variable for you and automatically rotates it.

    $ heroku addons:add securekey


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Added some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
