---
title: "Chapter 5: Testing setup"
date: '2012-11-13'
description:
categories:
---

Books have been written on how to test, why to test, and the advantages of testing.  Some developers like to test, others do not.  Testing tends to be fairly popular in the Railsl community.  But I'm not dogmatic.  Sometimes it is appropriate to test, other times it is not.  For this application, however, I'm going to be using tests.  A lot of tests.  And I'm going to be using test first development because I believe it's easier to develop that way.  I also believe that for this particular application, which I know will require me to write code that prcesses incoming emails, not testing is, well, not an option.  The idea of having to create a new email address to test confirmation manually, or of having to manually send an email every time I want do do a number of things is not an appealing one.  

Why Turnip?
==========

Some people hate Cucumber.  One of those people is [David Heinemeier Hannson](http://37signals.com/svn/posts/3159-testing-like-the-tsa). Of those who don't hate it, many more believe it is nire useful for teams that combine programmers with non-programmers so that everyone on the team understands where things are going, rather than to a programmer working by herself.  Or at least that's what [Matt Wynne](http://blog.mattwynne.net/2012/11/02/is-cucumber-just-a-scam/) seems to be saying.  I like it quite a bit, or at least I like the gherkin syntax, and in any case since many of the cucumber steps will be using either rspec or capybara, the learning curve isn't that steep.  Where gherkin shines is anywhere you're using lots of capyabara.  Or someplace you're going to be hitting different models and controllers.  Perhaps the place I like to use it the most is when I don't have the time or desire to understand how the code inside a particular gem I'm using works.  The best example of this is Devise.  I don't really care how Devise works, only that other people have tested it.  Devise is kind of a black box for me.  I click buttons and do signup and it generates migrations and adds columns to my user tables and sends emails out for confirmations.  So we'll use cucumber on it.

Rspec
=====
Some people prefer Test::Unit.  I'm open to the idea that Test::Unit is a more compact and faster test framework than Rspec.  But I really don't care.  Rspec is well documented and that counts for a lot.

Shoulda
=======
I tend to write a lot of tests some of which, in retrospect, turn out not to be imporant, or even redundant.  I test for the existence of database columns, model validations, pretty much everything I can think of.  The danger of over testing is almost exclusively that of wasting time.  Specifically, while there is always some value to writing a test, that value may not always equal the amount of time you spend writing it.  Fortunately, a lot of tests I write share similar structure between different applications, and writing a test today doesn't take nearly as long as it did when I was, well, still trying to figure out how to write tests.  Some of this is achieved by memorizing the structure of tests, or having several projects up on Github I can consult to see what I tested on a different project, as well as how I tested it.  But also it helps that I've figured out a lot of shorthands.  And Shoulda helps me to write them even more quickly. 

Email Spec
==========
This is a terrific library for testing incoming and outgoing emails.  If all you need to do is test outgoing emails, this is probably a winner.  If you must also test incoming emails, then it may pay off to roll your own -- as we will be doing in this tutorial.

Rather than go through the installation steps for each of these, I'll simply post a copy of my Gemfile, and then copies of the additional files we'll need.

*Gemfile*

    source 'https://rubygems.org'

    gem 'rails', '3.2.8'
    gem 'therubyracer', '0.10.2', :group => :assets, :platform => :ruby
    gem 'thin', '1.5.0'
    gem 'sendgrid', '1.0.1'
    gem 'devise', '2.1.2'
    gem 'simple_form', '2.0.4'
    gem 'haml', '3.1.7'
    gem 'jquery-rails', '2.1.3'
    gem 'bootstrap-sass', '2.1.0.1'
    gem 'font-awesome-rails', '0.4.1'
    gem 'faker', '1.1.2'

    group :assets do
      gem 'sass-rails',   '3.2.3'
      gem 'coffee-rails', '3.2.1'
      gem 'uglifier', '1.0.3'
    end

    group :development do 
      gem 'sqlite3', '1.3.6'
      gem 'haml-rails', '0.3.5'
      gem 'hpricot', '0.8.6'
      gem 'ruby_parser', '2.3.1'
      gem 'hub', '1.10.2', require: nil
    end

    group :test do 
      
      # test server
      gem 'spork', '0.9.2'
      
      # acceptance tests
      gem 'turnip', '1.0.0'
      gem 'email_spec', '1.2.1'
      gem 'shoulda-matchers', '1.4.1'
      gem 'capybara', '1.1.3'
      gem 'capybara-webkit', '0.13.0'
      gem 'selenium-webdriver', '2.26.0'
      gem 'database_cleaner', '0.9.1'
      gem 'launchy', '2.1.2'
      gem 'headless', '0.3.1'

     # Guards
      gem 'guard', '1.5.4'
      gem 'guard-spork', '1.2.3'
      gem 'guard-rspec', '2.1.1'
      gem 'guard-bundler', '1.0.0'
      gem 'guard-livereload', '1.1.0'
      gem 'guard-rails', '0.1.1'
      gem 'guard-sass', '1.0.1', require: false
    end

    group :test, :development do 
    gem 'rspec-rails', '2.11.4'
    gem 'factory_girl_rails', '4.1.0' 
    end

    group :production do
      gem 'pg'
    end

Now let's set up our spec_helper file:

*spec/spec_helper.rb*

    require 'rubygems'
    require 'spork'

    Spork.prefork do

      ENV["RAILS_ENV"] ||= 'test'
      require File.expand_path("../../config/environment", __FILE__)
      require 'rspec/rails'
      require 'rspec/autorun'
      require 'capybara/rspec'
      require 'capybara/rails'
      require 'factory_girl_rails'
      require 'shoulda-matchers'
      require 'email_spec'
      require 'turnip/capybara'
      require 'turnip_helper'

      Capybara.javascript_driver = :webkit

      # Requires supporting files with custom matchers and macros, etc,
      # in ./support/ and its subdirectories.
      Dir[Rails.root.join("spec/support/**/*.rb")].each {|f| require f}

      RSpec.configure do |config|
        config.include Devise::TestHelpers, :type => :controller
        config.include Warden::Test::Helpers
        config.include EmailSpec::Helpers
        config.include EmailSpec::Matchers
        # config.include ::Rails.application.routes.url_helpers

        config.include FactoryGirl::Syntax::Methods
        config.include EmailMacros

        config.mock_with :rspec
        config.fixture_path = "#{::Rails.root}/spec/fixtures"
        config.use_transactional_fixtures = false
        config.infer_base_class_for_anonymous_controllers = false
        
        config.before(:suite) do
          DatabaseCleaner.strategy = :transaction
          DatabaseCleaner.clean_with :truncation
        end

        config.before(:each) do
          DatabaseCleaner.start
        end

        config.after(:each) do
          DatabaseCleaner.clean
        end
      end
    end

    Spork.each_run do
      
      FactoryGirl.reload

      Dir["#{Rails.root}/app/controllers//*.rb"].each do |controller|
        load controller
      end
      Dir["#{Rails.root}/app/models//*.rb"].each do |model|
        load model
      end
      Dir["#{Rails.root}/app/workers//*.rb"].each do |worker|
        load worker
      end
      Dir["#{Rails.root}/app/mailers//*.rb"].each do |mailer|
        load mailer
      end
      Dir["#{Rails.root}/lib//*.rb"].each do |lib|
        load lib
      end
    end

Now lets set up our turnip helper:

*spec/turnip_helper.rb*

    Dir[Rails.root.join("spec/acceptance/steps/**/*.rb")].each {|f| require f}
    Dir[Rails.root.join("spec/acceptance/steps*.rb")].each {|f| require f}
    Dir[Rails.root.join("lib/turnip/*.rb")].each {|f| require f}

    RSpec.configure do |config|
      ## This is where we will include our steps for our turnip acceptance 
      ## steps, similar to cucumber step definition files, like this:
      # config.include AuthenticationSteps
    end

Now lets set up our Guardfile:

*Guardfile*

    require 'rbconfig'
    HOST_OS = RbConfig::CONFIG['host_os']

    group "background" do
      
      guard 'spork', bundler: true, 
        rspec_env: { 'RAILS_ENV' => 'test' }, wait: 200 do
          watch('Gemfile.lock')
          watch('config/application.rb')
          watch('config/environment.rb')
          watch(%r{^config/environments/.+\.rb$})
          watch(%r{^config/initializers/.+\.rb$})
          watch('spec/spec_helper.rb')
          watch('spec/turnip_helper.rb')
          watch(%r{^spec/acceptance/steps/.+\.rb$})
          watch(%r{^lib/turnip/steps/.+\.rb$})
        end
      end

      # Run Bundle with each Gemfile edit
      guard 'bundler' do
        watch('Gemfile')
      end
      
      # Reload The Browser when changes are made.
      guard 'livereload' do
        watch(%r{app/.+\.(erb|haml)})
        watch(%r{app/helpers/.+\.rb})
        watch(%r{(public/|app/assets).+\.(css|js|html)})
        watch(%r{(app/assets/.+\.css)\.s[ac]ss}) { |m| m[1] }
        watch(%r{(app/assets/.+\.js)\.coffee})
        watch(%r{config/locales/.+\.yml})
    end

    group "tests" do

      # Rspec
      guard 'rspec', :turnip => true, all_on_start: false, all_after_pass: false  do

          watch(%r{^spec/acceptance/.+\.feature$})
          watch(%r{^spec/acceptance/steps/(.+)_steps\.rb$}) { 'spec/acceptance'}

          watch(%r{^spec/acceptance/steps/*/(.+)_steps\.rb$}) { |m| Dir[File.join("**/#{m[1]}.feature")][0] || 'features' }
      
        #rspec
          watch(%r{^spec/.+_spec\.rb$}) 
          watch(%r{^lib/(.+)\.rb$})     { |m| "spec/lib/#{m[1]}_spec.rb" }
          watch('spec/spec_helper.rb')    { "spec" }
          watch('spec/turnip_helper.rb')  { "spec/acceptance" }
          watch(%r{^app/(.+)\.rb$})                           { |m| "spec/#{m[1]}_spec.rb" }
          watch(%r{^app/(.*)(\.erb|\.haml)$})                 { |m| "spec/#{m[1]}#{m[2]}_spec.rb" }
          watch(%r{^app/controllers/(.+)_(controller)\.rb$})  { |m| ["spec/routing/#{m[1]}_routing_spec.rb", "spec/#{m[2]}s/#{m[1]}_#{m[2]}_spec.rb", "spec/acceptance/#{m[1]}_spec.rb"] }
          watch(%r{^spec/support/(.+)\.rb$})                  { "spec" }
          watch('config/routes.rb')                           { "spec/routing" }
          watch('app/controllers/application_controller.rb')  { "spec/controllers" }
      end

      # Capybara request specs
      watch(%r{^app/views/(.+)/.*\.(erb|haml)$})          { |m| "spec/requests/#{m[1]}_spec.rb" }

    end

Now lets remove the features directory, since we are using turnip instead of cucumber and I'm afraid that leaving it in there will screw things up:

`$ rm -rf features`

Now to verify that our testing setup works properly with guard, we can run:

`$ bundle exec rake db:test:prepare`

`$ bundle exec guard start`

Once guard has started, your terminal should look something like this:

    22:38:46 - INFO - Guard::RSpec is running

    [1] guard(main)>

I don't like my tests to run automatically.  I prefer to have them run when I tell it to, by hitting enter from the prompt in the terminal.  Go ahead and do that now, which should give you terminal output that looks like this:

    [1] guard(main)> 
    22:42:29 - INFO - Run all
    22:42:30 - INFO - Bundle already up-to-date

    22:42:30 - INFO - Running all specs
    ................

    Finished in 1.4 seconds
    16 examples, 0 failures

    [1] guard(main)> 

If all your 16 tests (created for us using app compser in spec/models/ and spec/controllers) are passing, you are good to go.

Now lets write some turnip features for the user model based on what we know.  Since signing in and signing out are two features most websites have, and you're almost certain to have implemented them previously, let's start with a signin feature.

*spec/acceptance/user/sign_in.feature*

  Feature: Sign in
    In order to get access to protected sections of the site
    As a user
    I want to sign in

    Background:
      Given there are no emails
      And I am on the home page
      And I follow the "Sign in" link

      Scenario: Unregistered user attempts to log in
        Given no user exists with an email of "unregistered@sender.com"
        And I fill in "Email" with "unregistered@sender.com"
        And I fill in "Password" with "notregisteredpassword"
        And I press "Sign in"
        Then I should see a notice with "Invalid email or password."
        
      Scenario: Registered user enters wrong password
        Given a user exists with an email of "registered@signuptester.com"
        Given I fill in "Email" with "registered@signuptester.com"
        And I fill in "Password" with "wrongpassword"
        And I press "Sign in"
        Then I should see a notice with "Invalid email or password."
      
      Scenario: Registered user signs in
        Given a user exists with an email of "registered@signuptester.com"
        And I fill in "Email" with "registered@signuptester.com"
        And I fill in "Password" with "password"
        And I press "Sign in"
        Then I should see a notice with "You are now logged in"

When we save this file, guard should automatically run it for us.  for two reasons.  The first is that the steps have not yet been written.  Let's fix that now.  I like to put steps that I use frequently into the lib directory.  Lets do that now.  First ssteps for authentication:

*lib/turnip/authentication_steps.rb*  

    module AuthenticationSteps

      step 'I am not logged in' do 
        Warden.test_mode!
        logout
      end

      step 'no user exists with an email of :email' do |email|
        User.find_by_email(email).should be nil
      end
      
      step 'a user does exist with an email :registered_email' do |registered_email|
        User.create(email: registered_email)
        User.find_by_email(registered_email).should_not be nil
        reset_mailer
      end

      step 'a user exists with an email of :registered_email' do |registered_email|
        user = User.create(email: registered_email, password: "password")
        user.confirm!
        User.find_by_email(registered_email).should_not be nil
        reset_mailer
      end
      
      step 'I am registered and logged in as a/an :user_type' do |user_type|
        @actor = FactoryGirl.create(user_type.to_sym)
        @actor.confirm!
        login_as(@actor)
      end

      step 'I am registered, confirmed and logged in as a/an :user_type' do |user_type|
        @actor = FactoryGirl.create(user_type.to_sym).confirm!
        @actor.confirm!
        login_as(@actor)
      end

      step 'I open the password reset email' do 
        open_email(@actor.email)
      end
    end

Now some common steps using Ben Mabey's email_spec gem for email handling:

lib/turnip/email_handling_steps.rb

    module EmailHandlingSteps

      step 'there are no emails' do 
        reset_mailer
      end

      step 'I follow the :link_name link in the email' do |link_name| 
        visit_in_email(link_name)
      end

      step ':email_addressee should not have an email with :email_subject' do |email_addressee, email_subject|
        find_email(email_addressee, with_subject: email_subject).should be nil
      end

      step ':email_addressee should have :count email with subject :email_subject' do |email_addressee, count, email_subject|
        unread_emails_for(email_addressee).select { |m| 
          m.subject =~ Regexp.new(Regexp.escape(email_subject)) 
          }.size.should == parse_email_count(count)
      end

      step ':email_addressee should have an email' do |email_addressee|
        email = find_email(email_addressee)
        email.subject.should eq "Welcome to MerciboQ! -- Please confirm your account for us."
      end

      step 'I open the email to :email_addressee' do |email_addressee|
        open_email(email_addressee)
      end
    end

And finally some, well, utility steps:

*lib/turnip/utility_steps.rb*

    module UtilitySteps 

      step 'I am on the home page' do
        visit root_url
      end

      step 'I am on the :path path' do |path|
        visit "/#{path}"
      end

      step 'show me the page' do 
        save_and_open_page
      end

      step 'show me the email' do 
        EmailSpec::EmailViewer::save_and_open_email(Mail::TestMailer.deliveries.last)
      end

      step 'I follow the :link link' do |link|
        click_link(link)
      end

      step 'I fill in :value for :key' do |value, key|
        fill_in key, with: value
      end

      step 'I fill in :key with :value' do |key, value|
        fill_in key, with: value
      end

      step 'the :key field should contain :value' do |key, value| 
        page.has_field?(key, with: value)
      end

      step 'I press :button' do |button|
        click_button(button)
      end

      step 'I should see a notice with :text' do |text|
        page.should have_content(text)
      end

      step 'I should see an error notice with :text' do |text|
        page.should have_content(text)
      end
    end

The second reason this feature should fail is that the feature, as written, specifies that the user should find a "Sign in" button.  Instead, it sees a "Login" link.  Let's fix that:

*app/views/layouts/_navigation.html.haml*

    = link_to "Anydiver", root_path, :class => 'brand'
    %ul.nav
      - if user_signed_in?
        %li
          = link_to 'Sign out', destroy_user_session_path, :method=>'delete'
      - else
        %li
          = link_to 'Sign in', new_user_session_path
      - if user_signed_in?
        %li
          = link_to 'Edit account', edit_user_registration_path
      - else
        %li
          = link_to 'Sign up', new_user_registration_path

Now let's write a feature for signout:

spec/acceptance/user/sign_out.feature

    Feature: Sign out

      Scenario: Logged in user signs out
        Given I am registered and logged in as a user
        And I am on the home page
        And I follow the "Sign out" link
        Then I should see a notice with "Signed out successfully"

This should pass, too.  Now for a slightly tricky part.  How do we test the user sign up feature, when that feature involves sending out a confirmation email after inital signup, and then having the user open that email up, click the confirmation link inside it, and then finish up the sign up process?  Here's the signup feature:

    Feature: User sign up
      
      Background:
        Given there are no emails
        And I am not logged in
        And no user exists with an email "unregistered@sender.com"
        And no user exists with an email "unregistered@recipient.com"
        And a user does exist with an email "registered@sender.com"
        And a user does exist with an email "registered@recipient.com"

      Scenario: Successful signup via website
        Given I am on the home page
        And I follow the "Sign up" link
        And I fill in "sarah.silverman@test.com" for user_email 
        And I fill in "password" for user_password
        And I fill in "password" for user_password_confirmation
        And I press "Sign up"
        Then I should see a notice with "a confirmation link has been sent"
        And "sarah.silverman@test.com" should have 1 email with subject "Confirmation instructions"
        When I open the email to "sarah.silverman@test.com"
        And I follow the confirmation link in the email
        Then I should see a notice with "Your account was successfully confirmed."

When we run this, it fails.  That is because we do not yet have a factory that represents our users in an unconfirmed state.  Let's modify our factory so that we have a confirmed user and an unconfirmed user, both of which inherit from the user class:

*spec/factories/users.rb*
 
    FactoryGirl.define do
      factory :user, aliases: [] do
        sequence(:email)      { |n| "user#{Time.now.to_i + n}@gtest.com" }
        sequence(:name)       { |n| "test_user#{Time.now.to_i + n}" }
        
        confirmed_at          Time.now - 1.minute
        password              "password"
        password_confirmation "password"
      end

      factory :confirmed_user, parent: :user do 
        confirmed_at Time.now
      end

      factory :unconfirmed_user, parent: :user do 
        confirmed_at          nil
      end
    end

And 

All our acceptance tests should be passing now.  Let's write two more features.  The first is for when a user loses her password.  

*spec/acceptance/user/request_password.feature*

    Feature: Request password reset
      In order log into my account
      As a registered user who has lost my password
      I want to reset my password

        Background:
          And I am not logged in
          And I am on the home page
          And I follow the "Sign in" link
          And I follow the "Forgot your password?" link

        Scenario: User attempts to reset password with the wrong email
          Given no user exists with an email of "sarahsilverman@wrongdomain.com"
          And I fill in "sarasilverman@WRONGDOMAIN.com" for "Email"
          And I press "Send me reset password instructions"
          Then I should see an error notice with "Email not found"

        Scenario: User resets her password with the correct email
          Given I am registered and confirmed as a user
          And I fill in my email address
          And I press "Send me reset password instructions"
          Then I should see a notice with "You will receive an email with instructions"
          And I should have a password reset email
          When I open the password reset email
          And I follow the "Change my password" link in the email
          And I fill in "please" for "user_password"
          And I fill in "please" for "user_password_confirmation"
          And I press "Change my password"
          Then I should see a notice with "Your password was changed successfully. You are now signed in."

The second is for when she can't find her confirmation email, and needs a new one sent to her email address:

*spec/acceptance/user/reconfirm

    Feature: Reconfirmation
      As a registered but unconfirmed user
      In order to confirm my account
      I want to request a new confirmation email be sent to my email address

      Scenario: Request new confirmation
        Given I am not logged in
        And I have an account but have not confirmed it
        And I am on the sign_in path
        And I follow the "Didn't receive confirmation instructions?" link
        And I fill in my email address
        And I press "Resend confirmation instructions"
        And I should have a reconfirmation email
        When I follow the confirmation link in the email
        Then I should see a notice with "Your account was successfully confirmed. You are now signed in."

Now lets round out our testing of existing devise functionality with some unit tests:

*spec/models/user_spec.rb*

    require 'spec_helper'

    describe User do
      
      it "should be a type of class" do
        User.should be_kind_of(Class) 
      end

      describe 'database' do

        describe 'columns' do 

          %w[reset_password_sent_at remember_created_at current_sign_in_at 
            last_sign_in_at confirmed_at confirmation_sent_at 
            created_at updated_at
            ].each do |column|
            it { should have_db_column(column.to_sym).of_type(:datetime) }
          end

          %w[name email encrypted_password reset_password_token current_sign_in_ip 
            last_sign_in_ip confirmation_token unconfirmed_email
            ].each do |column|
            it { should have_db_column(column.to_sym).of_type(:string) }
          end

          it { should have_db_column(:sign_in_count).of_type(:integer) }
        end

        describe 'indexes' do 

          %w[email reset_password_token
          ].each do |index|
            it { should have_db_index(index.to_sym).unique(true)}
          end
        end
      end

      describe "security" do

        describe "mass assignable" do 

          %w[name email password password_confirmation
            remember_me
          ].each do |attribute|
            it {should allow_mass_assignment_of(attribute.to_sym) }
          end
        end

        describe "protected" do 

          %w[user_id encrypted_password reset_password_token 
            reset_password_sent_at remember_created_at unconfirmed_email 
            sign_in_count current_sign_in_at last_sign_in_at
            confirmation_token confirmed_at confirmation_sent_at 
            unconfirmed_email
          ].each do |attribute|
            it {should_not allow_mass_assignment_of(attribute.to_sym) }
          end
        end
      end

      describe "validations" do 

        describe "email addresses" do 

          it { should validate_presence_of(:email)
            .with_message(/can't be blank/) }
          it { should validate_uniqueness_of(:email) 
            .with_message(/already been taken/) }

          it "should allow properly formed emails" do 
            ["anydiver@test.com", "pat@gmail.com", "user.period@yahoo.com", 
              "user_underscore@msn.hotmail.com"
            ].each do |good_email|
              should allow_value(good_email).for(:email) 
            end
          end 

          it "should disallow spammy or bad looking emails" do 
            ["fre_d@.com", "@gmail.com", "hotmail.com", "gmail", 
              "_net&gmail."
            ].each do |bad_email|
              should_not allow_value(bad_email).for(:email) 
            end
          end
        end
      end

      describe "passwords" do 

        it "should be more than 5 and less than 41 characters long" do 
          should_not allow_value("a" * 129).for(:password) 
          should_not allow_value("a" * 5).for(:password) 
          should allow_value("a" * 128).for(:password) 
          should allow_value("a" * 6).for(:password) 
        end
      end
    end

Whew.  Now that's a lot of lines of test code for not a lot of actual application code -- and even that application code was generated for us.  Some people might argue that this is overkill.  But I like overkill.  And now I know that if I ever want to customize devise -- if, say, I want to upgrade to a newer version, or add a new module, or change the sign up flow so that a user enters her email address without a password, and then chooses her password after hitting the confirmation link we send to her email, then I can change those tests to specify the new desired behavior and then make those changes without worrying that everything will blow up on me.  So I encourage you to write tests, and take advantage of tools like shoulda to write them quickly.  And if you don't want to write tests, there's more than one way to write a Rails application.  Rails works for you, not the other way around.


