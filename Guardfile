# A sample Guardfile
# More info at https://github.com/guard/guard#readme

## Uncomment and set this to only include directories you want to watch
# directories %w(app lib config test spec features) \
#  .select{|d| Dir.exists?(d) ? d : UI.warning("Directory #{d} does not exist")}

## Note: if you are using the `directories` clause above and you are not
## watching the project directory ('.'), then you will want to move
## the Guardfile to a watched dir and symlink it back, e.g.
#
#  $ mkdir config
#  $ mv Guardfile config/
#  $ ln -s config/Guardfile .
#
# and, you'll have to watch "config/Guardfile" instead of "Guardfile"

# Defines the matching rules for Guard.
guard :minitest, spring: true, all_on_start: false do
  watch(%r{^test/(.*)/?(.*)_test\.rb$})
  watch(%r{^test/test_helper\.rb$})                 { 'test' }
  watch('config/routes.rb')                         { integration_tests }
  watch(%r{^app/models/(.*)\.rb$})                  { |m| "test/models/#{m[1]}_test.rb" }
  watch(%r{^app/controllers/(.*?)_controller\.rb$}) { |m| resource_tests(m[1]) }
  watch(%r{^app/views/([^/]*?)/.*\.html\.erb$})     { |m| ["test/controllers/#{m[1]}_controller_test.rb"] + integration_tests(m[1]) }
  watch(%r{^app/helpers/(.*?)_helper\.rb$})         { |m| integration_tests(m[1]) }

  watch('app/views/layouts/application.html.erb')   { 'test/integration/site_layout_test.rb' }
  # watch('app/helpers/sessions_helper.rb')           { integration_tests << 'test/helpers/sessions_helper_test.rb' }

  # watch('app/controllers/sessions_controller.rb') do
  #   ['test/controllers/sessions_controller_test.rb',
  #    'test/integration/users_login_test.rb']
  # end
  # watch('app/controllers/account_activations_controller.rb') { 'test/integration/users_signup_test.rb' }

  # watch(%r{^app/views/users/*$}) { resource_tests('users') + ['test/integration/microposts_interface_test.rb'] }
end

# Returns the integration tests corresponding to the given resource.
def integration_tests(resource = :all)
  if resource == :all
    Dir["test/integration/*"]
  else
    Dir["test/integration/#{resource}_*.rb"]
  end
end

# Returns the controller tests corresponding to the given resource.
def controller_test(resource)
  "test/controllers/#{resource}_controller_test.rb"
end

# Returns all tests for the given resource.
def resource_tests(resource)
  integration_tests(resource) << controller_test(resource)
end
