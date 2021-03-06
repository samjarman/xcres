#encoding: utf-8

begin
  require 'bundler/gem_tasks'
  require 'rubocop/rake_task'

  namespace :install do
    desc 'Install the gem as current user (NOT RECOMMENDED)'
    task :user => ['build'] do
      sh "gem install #{Dir['pkg/*.gem'].sort.last} --user-install"
    end
  end

  namespace :lint do
    RuboCop::RakeTask.new do |task|
      task.patterns = ['lib/**/*.rb']
    end

    desc 'Run inch to see documentation coverage'
    task :doc do
      sh 'bundle exec inch'
    end
  end

  namespace :spec do

    task :prepare do
      verbose false
      puts 'Prepare …'
      sh 'mkdir -p tmp'
      rm_rf 'tmp/*'
    end

    desc 'Run all integration specs'
    task :integration => [:prepare] do
      sh 'bundle exec bacon spec/integration.rb'
    end

    desc 'Run all unit specs'
    task :unit, [:spec] do |t, args|
      sh "bundle exec bacon #{specs(args[:spec] || '**/*')}"
    end

    def specs(dir)
      FileList["spec/unit/#{dir}_spec.rb"].shuffle.join(' ')
    end

    desc 'Run all specs'
    task :all => [
      :unit,
      :integration
    ]

  end

  desc 'Run all specs'
  task :spec => 'spec:all'
end
