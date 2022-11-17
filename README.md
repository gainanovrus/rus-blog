
## Prerequisites
- Ruby version 2.5.0 or higher
- RubyGems
- Jekyll

## Local build and run

### With Docker

Run in terminal
```
docker-compose up
```

That build all pages and run http-server on the 4000 port. See a result of the command
```
rus_blog  |             Source: /site
rus_blog  |        Destination: /site/_site
rus_blog  |  Incremental build: disabled. Enable with --incremental
rus_blog  |       Generating...
rus_blog  |        Jekyll Feed: Generating feed for posts
rus_blog  |                     done in 2.654 seconds.
rus_blog  |  Auto-regeneration: enabled for '/site'
rus_blog  |     Server address: http://0.0.0.0:4000/rus-blog/
rus_blog  |   Server running... press ctrl-c to stop.
```

Open deployment in browser ([localhost:4000/rus-blog](http:/localhost:4000/rus-blog))

Or install locally all required components without Docker.

### Without Docker

#### Install ruquired utils

When you use Fedora 30
```
sudo dnf install ruby ruby-devel rubygems-devel \
                 autoconf automake bison gcc-c++ libffi-devel libtool \
                 libyaml-devel readline-devel sqlite-devel zlib-devel \
                 openssl-devel redhat-rpm-config rubygem-nokogiri

gem install jekyll bundler
bundler update --bundler
```

Uses Ubuntu 20.04 and later
```
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

gem install jekyll bundler
bundler update --bundler
```

On MacOS
```
brew update
brew install ruby
echo 'export PATH="/usr/local/opt/ruby/bin:$PATH"' >> ~/.zshrc

gem install jekyll bundler
bundler update --bundler
```

Also see a [jekyll documentation](https://jekyllrb.com/docs/)

#### Build and run site

Deploy a site on localhost
```
bundler install
bundle exec jekyll serve --config=_config.yml,_config.dev.yml --incremental --verbose --watch
```

Open deployment in browser ([localhost:4000/rus-blog](http:/localhost:4000/rus-blog))

## Deploy to GitHub

Site was deployed automaticaly on push to master branch. Uses GitHub Actions. See a [results](https://github.com/gainanovrus/rus-blog/actions/workflows/pages.yml)

## Open and test

URL: [gainanov.pro/rus-blog](https://gainanov.pro/rus-blog)
