Heroku Ruby Jekyll Buildpack
============================

Heroku Ruby Jekyll Buildpack is a fork of Heroku's [official Ruby buildpack](https://github.com/heroku/heroku-buildpack-ruby) with added support for generating static [Jekyll](https://github.com/mojombo/jekyll) sites during the build/deployment stage.

With this [buildpack](http://devcenter.heroku.com/articles/buildpacks) you no longer need pre-build the site or commit the _site build directory to your repo. This simplifies the deployment process and keeps the repo clean. All of the standard Ruby tools are maintained in this buildpack, so you can take full advantage of Rack middleware and other useful tools from the Ruby ecosystem.

Usage
-----

Create a new Cedar-stack app with this buildpack
=======
### Ruby

Example Usage:

    $ ls
    Gemfile Gemfile.lock

    $ heroku create --stack cedar --buildpack https://github.com/heroku/heroku-buildpack-ruby.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Ruby app detected
    -----> Installing dependencies using Bundler version 1.1.rc
           Running: bundle install --without development:test --path vendor/bundle --deployment
           Fetching gem metadata from http://rubygems.org/..
           Installing rack (1.3.5)
           Using bundler (1.1.rc)
           Your bundle is complete! It was installed into ./vendor/bundle
           Cleaning up the bundler cache.
    -----> Discovering process types
           Procfile declares types -> (none)
           Default types for Ruby  -> console, rake

The buildpack will detect your app as Ruby if it has a `Gemfile` and `Gemfile.lock` files in the root directory. It will then proceed to run `bundle install` after setting up the appropriate environment for [ruby](http://ruby-lang.org) and [Bundler](http://gembundler.com).

#### Bundler

For non-windows `Gemfile.lock` files, the `--deployment` flag will be used. In the case of windows, the Gemfile.lock will be deleted and Bundler will do a full resolve so native gems are handled properly. The `vendor/bundle` directory is cached between builds to allow for faster `bundle install` times. `bundle clean` is used to ensure no stale gems are stored between builds.


    heroku create -s cedar --buildpack http://github.com/mattmanning/heroku-buildpack-ruby-jekyll.git

or add this buildpack to your current app

    heroku config:add BUILDPACK_URL=http://github.com/mattmanning/heroku-buildpack-ruby-jekyll.git

Create a Ruby web app with dependencies managed by [Bundler](http://gembundler.com/) and a Jekyll site. [Heroku-Jekyll-Hello-World](https://github.com/burkemw3/Heroku-Jekyll-Hello-World) can be used as a sample starter.

Push to heroku
=======

    git push heroku master

Watch it "Building jekyll site"

Example Usage:

    $ ls
    app  config  config.ru  db  doc  Gemfile  Gemfile.lock  lib  log  Procfile  public  Rakefile  README  script  tmp  vendor

    $ ls config/application.rb
    config/application.rb

    $ heroku create --stack cedar --buildpack https://github.com/heroku/heroku-buildpack-ruby.git

    $ git push heroku master
    -----> Heroku receiving push
    -----> Fetching custom build pack... done
    -----> Ruby/Rack app detected
    -----> Installing dependencies using Bundler version 1.1.rc
           Running: bundle install --without development:test --path vendor/bundle --binstubs bin/ --deployment
           Fetching gem metadata from http://rubygems.org/.......
           Using RedCloth (4.2.8)
           Using posix-spawn (0.3.6)
           Using albino (1.3.3)
           Using fast-stemmer (1.0.0)
           Using classifier (1.3.3)
           Using daemons (1.1.4)
           Using directory_watcher (1.4.1)
           Using eventmachine (0.12.10)
           Using kramdown (0.13.3)
           Using liquid (2.3.0)
           Using syntax (1.0.0)
           Using maruku (0.6.0)
           Using jekyll (0.11.0)
           Using rack (1.3.5)
           Using thin (1.3.1)
           Using bundler (1.1.rc)
           Your bundle is complete! It was installed into ./vendor/bundle
           Cleaning up the bundler cache.
           Building jekyll site
    -----> Discovering process types
           Procfile declares types     -> web
           Default types for Ruby/Rack -> console, rake
    -----> Compiled slug size is 7.2MB
    -----> Launching... done, v47
    -----> Deploy hooks scheduled, check output in your logs
           http://www.mwmanning.com deployed to Heroku

    To git@heroku.com:mattmanning.git
       8f84bc4..9350a12  master -> master

See Also
--------

The blog post introducing this buildpack: [http://mwmanning.com/2011/11/29/Run-Your-Jekyll-Site-On-Heroku.html](http://mwmanning.com/2011/11/29/Run-Your-Jekyll-Site-On-Heroku.html).

