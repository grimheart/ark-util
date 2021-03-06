require 'rubygems/package_task'
require 'rdoc/task'

v = `git describe --tags`.strip.tr('-', '.')
c = 2 - v.count('.')
if c > 0
  v = v + ('.0' * c)
else
  v.sub!(/\.[^\.]+$/, '')
end
if !`git status --porcelain`.empty?
  v = v + '.dev'
end
Version = v

spec = Gem::Specification.new do |s|
  s.platform = Gem::Platform::RUBY
  s.name     = 'ark-util'
  s.version  = Version
  s.license  = 'GPL-3.0'
  s.summary  = "Utility library for ark-* gems"
  s.authors  = ["Macquarie Sharpless"]
  s.email    = ["macquarie.sharpless@gmail.com"]
  s.homepage = "https://github.com/grimheart/ark-util"
  s.description = s.summary

  s.require_paths = ['lib']
  s.files = Dir['lib/ark/*']
end

desc "Print the version for the current revision"
task :version do
  puts Version
end

desc "Open an IRB session with the library already require'd"
task :console do
  require 'irb'
  require 'irb/completion'
  require_relative 'lib/ark/utility.rb'
  ARGV.clear
  IRB.start
end

desc "Run all test cases"
task :test do
  Dir['test/*'].select {|p| File.basename(p)[/^tc_.+\.rb$/] }.each do |path|
    system "ruby #{path}"
  end
end

desc "Build a gem then install"
task :install => [:clobber, :gem] do
  system "gem install pkg/#{spec.name}-#{Version}.gem"
end

desc "Push a gem to rubygems.org"
task :push => :gem do
  system "gem push pkg/#{spec.name}-#{Version}.gem"
end

Gem::PackageTask.new(spec) do |pkg|
  pkg.need_zip = true
  pkg.need_tar = true
end

Rake::RDocTask.new do |rd|
  rd.main       = 'README.md'
  rd.rdoc_dir   = 'doc'
  rd.title      = "ark-util #{Version} Documentation"
  rd.rdoc_files.include("README.md", "lib/ark/utility.rb")
end

