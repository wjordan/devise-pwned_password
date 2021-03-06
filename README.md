# Devise::PwnedPassword
Devise extension that checks user passwords against the PwnedPasswords dataset https://haveibeenpwned.com/Passwords

Based on

https://github.com/HCLarsen/devise-uncommon_password


## Usage
Add the :pwned_password module to your existing Devise model.

```ruby
class AdminUser < ApplicationRecord
  devise :database_authenticatable, 
         :recoverable, :rememberable, :trackable, :validatable, :pwned_password
end
```

Users will receive the following error message if they use a password from the
PwnedPasswords dataset:

```
This password has previously appeared in a data breach and should never be used. Please choose something harder to guess.
```

By default passwords are rejected if they appear at all in the data set.
Optionally, you can add the following snippet to `config/initializers/devise.rb`
if you want the error message to be displayed only when the password is present
a certain number of times in the data set:

```ruby
# Minimum number of times a pwned password must exist in the data set in order
# to be reject.
config.min_password_matches = 10
```

## Installation
Add this line to your application's Gemfile:

```ruby
gem 'devise-pwned_password'
```

And then execute:
```bash
$ bundle install
```


## Considerations

A few things to consider/understand when using this gem:

* User passwords are hashed using SHA-1 and then truncated to 5 characters,
  implementing the k-Anonymity model described in
  https://haveibeenpwned.com/API/v2#SearchingPwnedPasswordsByRange
  Neither the clear-text password nor the full password hash is ever transmitted
  to a third party. More implementation details and important caveats can be
  found in https://blog.cloudflare.com/validating-leaked-passwords-with-k-anonymity/

* This puts an external API in the request path of users signing up to your
  application. This could potentially add some latency to this operation. The
  gem is designed to fail silently if the PwnedPasswords service is unavailable.

## Contributing

To contribute

* Check the [issue tracker](https://github.com/michaelbanfield/devise-pwned_password/issues) and [pull requests](https://github.com/michaelbanfield/devise-pwned_password/pulls) for anything similar
* Fork the repository
* Make your changes
* Run bin/test to make sure the unit tests still run
* Send a pull requests

## License
The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).
