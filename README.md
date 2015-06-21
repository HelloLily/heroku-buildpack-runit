# heroku-buildpack-runit

A buildpack to run mulitple processes within a dyno via [runit](http://smarden.org/runit/).

# Why a HelloLily Fork

To make sure buildpack exists, because it isn't an official Heroku buildpack.

## Why?

Imagine you want to run nginx in your `web` dynos, binding to `$PORT`, and fronting unicorn. You want them to run individually as if they were specified directly in the `Procfile` (including output to the log stream) without worrying about daemonizing.

## Usage

You'll probably use this in conjunction with [the multi buildpack](https://github.com/ddollar/heroku-buildpack-multi).

For every process type (eg `web`) you want to run mulitple processes in, add an entry like this to your `Procfile`:

```
web: bin/runsvdir-dyno
```

Then, create a `Procfile.web` file (where `web` matches the process type in `Procfile`). It might look like this:

```
nginx: bin/nginx-runner -p $PORT ...
unicorn: bundle exec unicorn -c config/unicorn.rb ...
```

Now, when running a `web` dyno, both the `nginx` and `unicorn` processes will run under runit.

If any process specified in `Procfile.web` crashes, the entire dyno will be restarted.
