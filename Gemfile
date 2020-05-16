source "https://rubygems.org"

git_source(:github) {|repo_name| "https://github.com/#{repo_name}" }

gem "jekyll", "~> 3.8.4"

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw, :jruby]
# file change watcher for windows
gem 'wdm', '>= 0.1.0' if Gem.win_platform?

group :jekyll_plugins do
    gem "jekyll-menus", "~> 0.6.0"
    gem "jekyll-paginate-v2", "~> 1.9"
    gem "jekyll-paginate", "~> 1.1"
end

# run "bundle exec jekyll serve" to develop locally
