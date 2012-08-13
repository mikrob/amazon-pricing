require 'bundler'
require 'rake/testtask'
require 'lib/amazon-pricing/version'

Rake::TestTask.new(:test) do |test|
  test.libs << 'lib' << 'test'
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end


desc "Build the gem"
task :gem do
  sh 'gem build *.gemspec'
end

desc "Publish the gem"
task :publish do
  sh "gem push amazon-pricing-#{AwsPricing::VERSION}.gem"
end

desc "Installs the gem"
task :install => :gem do
  sh "#{SUDO} gem install amazon-pricing.gem --no-rdoc --no-ri"
end

task :test do
  ruby "test/test-ec2-instance-types.rb"
end

desc "Prints current EC2 pricing to console"
task :print_price_list do
  require 'lib/amazon-pricing'
  pricing = AwsPricing::PriceList.new
  puts "Region,Instance Type,Linux PPH,Windows PPH"
  pricing.regions.each do |region|
    region.ec2_on_demand_instance_types.each do |t|
      puts "#{region.name},on-demand,#{t.api_name},#{t.linux_price_per_hour},#{t.windows_price_per_hour}"
    end
    region.ec2_reserved_instance_types.each do |t|
      puts "#{region.name},#{t.usage_type},#{t.api_name},#{t.linux_price_per_hour},#{t.windows_price_per_hour}"
    end
  end
end

task :default => [:test]
