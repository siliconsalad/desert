require "rake"
require 'rake/contrib/rubyforgepublisher'
require 'rake/clean'
require 'rake/testtask'
require 'rdoc/task'

desc "Runs the Rspec suite in sqlite (the default)"
task :default => "spec:sqlite"

desc "Runs the Rspec suite in sqlite (the default)"
task :spec => :default

namespace :spec do
  task :default => :sqlite

  desc "Runs the Rspec suite in sqlite (the default)"
  task :sqlite do
    run_suite
  end

  desc "Runs the Rspec suite in all databases"
  task :all_databases do
    ENV["DATABASES_TO_TEST"] = "sqlite,postgres"
    run_suite
  end
end

def run_suite
  dir = File.dirname(__FILE__)
  system("ruby #{dir}/spec/spec_suite.rb") || raise("Example Suite failed")
end

begin
  gem "jeweler"
  require 'jeweler'
  Jeweler::Tasks.new do |s|
    s.name = "desert"
    s.summary = "Desert is a component framework for Rails that allows your plugins to be packaged as mini Rails apps."
    s.email = "opensource@pivotallabs.com"
    s.homepage = "http://pivotallabs.com"
    s.description = "Desert is a component framework for Rails that allows your plugins to be packaged as mini Rails apps."
    s.authors = ["Pivotal Labs", "Brian Takita", "Parker Thompson", "Adam Milligan, Joe Moore"]
    s.files =  FileList[
      '[A-Z]*',
      '*.rb',
      'lib/**/*.rb',
      'generators/**/*',
      'generators/**/templates/*',
      'examples/**/*.rb'
    ].to_a
    s.extra_rdoc_files = [ "README.rdoc", "CHANGES" ]
    s.rdoc_options = ["--main", "README.rdoc", "--inline-source", "--line-numbers"]
    s.test_files = Dir.glob('spec/*_spec.rb')
    s.rubyforge_project = "desert"
  end
rescue LoadError
  puts "Jeweler, or one of its dependencies, is not available. Install it with: sudo gem install technicalpickles-jeweler -s http://gems.github.com"
end


desc "Install dependencies to run the build. This task uses Git."
task :install_dependencies do
  require "lib/desert/supported_rails_versions"
  system("git clone git://github.com/rails/rails.git /Users/pwoestelandt/workspace/desert/spec/rails_root/vendor/rails_versions/edge")
  Dir.chdir("spec/rails_root/vendor/rails_versions/edge") do
    begin
      Desert::SUPPORTED_RAILS_VERSIONS.each do |version, data|
        unless version == 'edge'
          system("git checkout #{data['git_tag']}")
          system("cp -R ../edge ../#{version}")
        end
      end
    ensure
      system("git checkout master")
    end
  end
end

desc "Updates the dependencies to run the build. This task uses Git."
task :update_dependencies do
  system "cd spec/rails_root/vendor/rails_versions/edge; git pull origin"
end

desc "Runs the CI build"
task :cruise => :install_dependencies do
  Rake::Task["spec:sqlite"].execute #TODO: change to spec:all_databases
end

