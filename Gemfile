# frozen_string_literal: true

source "https://rubygems.org"

gemspec
source "https://rubygems.org"

gem "jekyll", "~> 4.3.0"
gem "github-pages", group: :jekyll_plugins  # Asegúrate de incluir esta gema si usas GitHub Pages
gem "jekyll-theme-chirpy"  # Si usas el tema Chirpy

gem "html-proofer", "~> 5.0", group: :test

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]
