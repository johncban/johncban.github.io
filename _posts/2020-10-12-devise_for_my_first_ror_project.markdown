---
layout: post
title:      "Devise for My First RoR Project"
date:       2020-10-12 07:20:21 -0400
permalink:  devise_for_my_first_ror_project
---

Devise is another Rails authentication available from Ruby gems library, its the most common authentication gem because of the following factors:
* Rack based.
* Complete solution for MVC because it’s based in Rails.
* Models Flexibility.
* Simple due to modularity.

I personally use Devise rather than creating my own Rails Authentication for ease and security. 
It was easy to deploy (it will be explained set-by-step) as mentioned before because of its modularity and most of all its secure because of explicit routes, and optional OmniAuth deployment.

## Creation and Installation
To get started we need to create a project, in my case I have to start creating my Rails project Alpha version called LiveWire.


`rails new livewire_alpha`


Then in the Gemfile inside the project, add devise gem then install using bundle in the terminal.


`gem 'devise'`

`bundle install`  


After the bundle process, it requires to install generate devise by using the following command:

`rails g devise:install`  


Refractor devise’s environment configuration for your environment or server IP address in `config/environment/development.rb`

`config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }`

Another thing, in the above’s environment configuration the port can be change but it depends on the server environment you have as per port availability.

## Deployment
Now its time to install devise, this next step will generate the important parts of devise to the Rails project.
Generate a devise model by typing the following command, but make sure the “MODEL” is replaced either User or Admin but my project’s case I rather use User.

`rails g devise user. ` 

Consequently, it will generate the following:
```
Running via Spring preloader in process 16815
      invoke  active_record
      create    db/migrate/20201012071614_devise_create_users.rb
      create    app/models/user.rb
      invoke    test_unit
      create      test/models/user_test.rb
      create      test/fixtures/users.yml
      insert    app/models/user.rb
       route  devise_for :users
```

Next in order to create a table for users migration will be needed.

`rails db:migrate`

It will prompt the following results:
```
== 20201012085921 DeviseCreateUsers: migrating ================================
-- create_table(:users)
   -> 0.0027s
-- add_index(:users, :email, {:unique=>true})
   -> 0.0011s
-- add_index(:users, :reset_password_token, {:unique=>true})
   -> 0.0011s
== 20201012085921 DeviseCreateUsers: migrated (0.0051s) =======================
```

Before running the server, I have to refractor the routes in `config/routes.rb` by doing the following:
```
Rails.application.routes.draw do
  devise_for :users 
  authenticated :user do
    root 'user_dashboard#index', as: :authenticated_root
  end
  root 'main#index'
end
```

Next step is to generate a controller to support my main page and user dashboard with its views.

## Controller and Views
In the previous section devise setup was started now its the controller and views, including devise but this time its all about views.

I generated main controller and user_dashboard controller by doing the following:
`rails g controller main` and `rails g controller user_dashboard`

Inside `main_controller.rb`, I refractor it with index so the route view `main#index` can be recognize. However, `user_dashboard_controller.rb` was not refractored (for now).

The user dashboard views or `user_dashboard\index.html.erb` have the following contents:
```
<% if user_signed_in? %>
            <%= link_to('Logout', destroy_user_session_path, method: :delete) %>
          <% else %>
            <%= link_to('Login', new_user_session_path) %>
<% end %>
```
To assist the user to log-out; otherwise, log-in.

As for views, I added index.html.erb in both main and user_dasboard folder view but in devise I have to use the following command:

`rails g devise:views`

The main reason of generating devise views is for the developer (or you) to have visibility of the form either for cuszomization purposes or for rendering.

Now to test devise, run the rails app by typing `rails s` in your terminal and it will reforward the page to main then as you click login the address will turn into `http://localhost:3000/users/sign_in` to login. As for signup, the main address is `http://localhost:3000/users/sign_up`

I personally suggest to check my resources behind the initiative of devise gem to my project, for some it seems challenging but just like mentors say "mistakes is your best mentor".


### Resources:
- https://github.com/heartcombo/devise
- https://stevepolito.design/blog/rails-react-tutorial/







