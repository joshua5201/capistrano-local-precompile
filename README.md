# Capistrano Local Precompile

If your Rails apps are anything like mine, one of the slowest parts of your deployment is waiting for asset pipeline precompilation. It's sometimes so slow, it's painful. So I went searching for some solutions. [turbo-sprockets](https://github.com/ndbroadbent/turbo-sprockets-rails3) helped, but it's not a silver bullet.  This gem isn't a silver bullet either, but it can help.  Capistrano Local Precompile takes a different approach. It builds your assets locally and rsync's them to your web server(s).

This branch is a modification for uploading to google cloud storage after precompiling.

## Usage

Add capistrano-local-precompile to your Gemfile:

```ruby
group :development do
  # Capistrano v2 should use '~> 0.0.5'
  # Capistrano v3 should use '~> 1.0.0'
  # Capistrano v3.8+ should use '~> 1.1.1'
  gem 'capistrano-local-precompile', '~> 1.1.1', require: false
end
```

Then add the following line to your `Capfile`:

```ruby
require 'capistrano/local_precompile'
```

Remove the following line from your `Capfile`:

```ruby
require 'capistrano/rails/assets'
```

Here's the full set of configurable options:

```ruby
    # required
    set :gcloud_bucket,   "[your bucket]"

    # optional
    set :precompile_env,   fetch(:rails_env) || 'production'
    set :assets_dir,       "public/assets"
    set :packs_dir,        "public/packs"    
    set :remote_assets_dir, "assets"
    set :remote_packs_dir, "packs"
    set :rsync_cmd,        "gsutil -m rsync -d -r"
```

## Acknowledgement

This gem is derived from gists by [uhlenbrock][] and [keighl][].

[uhlenbrock]: https://gist.github.com/uhlenbrock/1477596
[keighl]: https://gist.github.com/keighl/4338134

## Contributing

Pull requests welcome: fork, make a topic branch, commit (squash when possible) *with tests* and I'll happily consider.

## Copyright

Copyright (c) 2017 Steve Agalloco / Tom Caflisch. See [LICENSE](LICENSE.md) for detail
