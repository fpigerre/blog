require 'html/proofer'

task :test do
  sh "bundle exec jekyll build"
  sh export NOKOGIRI_USE_SYSTEM_LIBRARIES=true
  HTML::Proofer.new("./_site").run
end

task :default => :test