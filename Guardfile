if Gem::Version.new(RUBY_VERSION) >= Gem::Version.new("3.2")
  warn "\n\n🚨 🚨 🚨  Check for newer version of guard-bundler which is ruby 3.2 compatible 🚨 🚨 🚨\n\n\n➡️  ➡️  ➡️   https://github.com/guard/guard-bundler ⬅️  ⬅️  ⬅️\n\n\n"
else
  guard :bundler do
    require "guard/bundler"
    require "guard/bundler/verify"
    helper = Guard::Bundler::Verify.new

    files = ["Gemfile"]
    files += Dir["*.gemspec"] if files.any? { |f| helper.uses_gemspec?(f) }

    # Assume files are symlinked from somewhere
    files.each { |file| watch(helper.real_path(file)) }
  end
end

# Note: The cmd option is now required due to the increasing number of ways
#       rspec may be run, below are examples of the most common uses.
#  * bundler: "bundle exec rspec"
#  * bundler binstubs: "bin/rspec"
#  * spring: "bin/rspec" (This will use spring if running and you have
#                          installed the spring binstubs per the docs)
#  * zeus: "zeus rspec" (requires the server to be started separately)
#  * "just" rspec: "rspec"

guard :rspec, cmd: "bundle exec rspec" do
  require "guard/rspec/dsl"
  dsl = Guard::RSpec::Dsl.new(self)

  # Feel free to open issues for suggestions and improvements

  # RSpec files
  rspec = dsl.rspec
  watch(rspec.spec_helper) { rspec.spec_dir }
  watch(rspec.spec_support) { rspec.spec_dir }
  watch(rspec.spec_files)

  # Ruby files
  ruby = dsl.ruby
  dsl.watch_spec_files_for(ruby.lib_files)
end
