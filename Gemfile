source "https://rubygems.org"

gem "logger"
gem "csv"
gem "base64"
gem "bigdecimal"
gem "jekyll-sass-converter", "~> 2.0"
gem "jekyll", "~> 4.3"
gem "minimal-mistakes-jekyll"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
  gem "jekyll-include-cache"
  gem "jekyll-paginate"
  gem "faraday-retry"
end

# Windows and JRuby does not include zoneinfo files
platforms :windows, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end
