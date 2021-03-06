[![Build Status](https://travis-ci.org/interagent/heroics.png?branch=master)](https://travis-ci.org/interagent/heroics)
# Heroics

Ruby HTTP client generator for APIs represented with JSON schema.

## Installation

Add this line to your application's Gemfile:

    gem 'heroics'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install heroics

## Usage

### Generating a client

Heroics generates an HTTP client from a JSON schema that describes your API.
Look at [prmd](https://github.com/interagent/prmd) for tooling to help write a
JSON schema.  When you have a JSON schema prepared you can generate a client
for your API:

```
bin/heroics-generate MyApp schema.json https://api.myapp.com > client.rb
```

### Passing custom headers

If your client needs to pass custom headers with each request these can be
specified using `-H`:

```
heroics-generate \
  -H "Accept: application/vnd.myapp+json; version=3" \
  MyApp \
  schema.json \
  https://api.myapp.com > client.rb
```

Pass multiple `-H` options if you need more than one custom header.

### Client-side caching

The generated client sends and caches ETags received from the server.  By
default, this data is cached in memory and is only used during the lifetime of
a single instance.  You can specify a directory for cache data:

```
heroics-generate \
  -c "~/.heroics/myapp" \
  MyApp \
  schema.json \
  https://api.myapp.com > client.rb
```

`~` will automatically be expanded to the user's home directory.  Be sure to
wrap such paths in quotes to avoid the shell expanding it to the directory you
built the client in.

### Generating API documentation

The generated client has [Yard](http://yardoc.org/)-compatible docstrings.
You can generate documentation using `yardoc`:

```
yard doc -m markdown client.rb
```

This will generate HTML in the `docs` directory.  Note that Yard creates an
`_index.html` page won't be served by Jekyll on GitHub Pages.  Add a
`.nojekyll` file to your project to prevent GitHub from passing the content
through Jekyll.

### Handling failures

The client uses [Excon](https://github.com/geemus/excon) under the hood and
raises Excon errors when failures occur.

```ruby
begin
  client.app.create({'name' => 'example'})
rescue Excon::Errors::Forbidden => error
  puts error
end
```

## Contributing

1. [Fork the repository](https://github.com/interagent/heroics/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new pull request
