# encoding: utf-8

require 'rake'
require 'rspec/core/rake_task'

task :spec    => 'spec:all'
task :default => :spec

namespace :spec do
    targets = []
    Dir.glob('./spec/*').each do |dir|
        next unless File.directory?(dir)
        targets << File.basename(dir)
    end

    task :all     => targets
    # task :default => :all

    targets.each do |target|
        desc "Run serverspec tests to #{target}"
        RSpec::Core::RakeTask.new(target.to_sym) do |t|
            ENV['TARGET_HOST'] = target
            t.pattern = "spec/#{target}/*_spec.rb"
        end
    end
end

desc 'Initial setup'
task :init do
    sh 'gem install bundler'
    sh 'bundle install --path vendor/bundle'
    sh 'vagrant plugin install vagrant-multiplug'
    sh 'ssh-add -K ~/.ssh/id_rsa'
    Rake::Task['install_roles'].invoke()
end

desc 'Install ansible roles from requirements.txt'
task :install_roles do
    sh "cd ./provisioning/ && ansible-galaxy install -r requirements.yml -p ./roles -f"
end
end
