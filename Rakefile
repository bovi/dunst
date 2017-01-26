require 'rubygems/package_task'
require 'rake/testtask'

spec = Gem::Specification.new do |s|
  s.name         = 'smog'
  s.version      = '0.0.1'
  s.date         = '2017-01-27'
  s.summary      = "Simple cloud management tool"
  s.description  = "Simple cloud management tool using SSH and VMWare Workstation. Supporting clone, start and stop of VMs."
  s.authors      = ["Daniel Bovensiepen"]
  s.email        = 'daniel@bovensiepen.net'
  s.files        = Dir.glob("{bin}/**/*") + %w(LICENSE README.md)
  s.executables  = ['hieb']
  s.add_runtime_dependency 'net-ssh', '~> 4.0', '>= 4.0.1'
  s.homepage     = 'https://github.com/bovi/smog'
  s.license      = 'MIT'
end

Gem::PackageTask.new(spec) do |pkg|
  pkg.need_zip = true
  pkg.need_tar = true
end

task :clean => [:clobber_package]
