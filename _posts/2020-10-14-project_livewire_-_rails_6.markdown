---
layout: post
title:      "Project LiveWire - Rails 6"
date:       2020-10-14 12:49:19 +0000
permalink:  project_livewire_-_rails_6
---


![LiveWire](https://rawcdn.githack.com/johncban/livewire/0806f62afecf3b3f9da856120a37849f6e968e3a/app/assets/images/logo/lw_logo.png)

This is my third project under Flatiron, it took me almost 2 months to make my idea possible by testing gems, researching the right API, testing the API again and meeting the requirements. It's called [LiveWire](https://github.com/johncban/livewire) (my project development is unpredictable), a semi-social media like that add users as friends and everyone can comment to users posts. But the unique part of my project is that users can publish or unpublish their post using scope, create a personal appointment then a COVID-19 API will provide information about the appointment location's infection rate if its safe to go or not. For privacy's sake, the appointment cannot be seen by other users but rather its exclusively for the owners. Without said privacy is hard to comeby but this project have still room for growth to be a true (useful) social media app.

<p align="center">
  <img width="460" height="300" src="https://media.giphy.com/media/9SN6sjZZ3XzB6/giphy.gif">
</p>

#### Gems

LiveWire's gems are pretty common but unique in many ways and it make my project development easier. The main key gems in this project are:
1. [acts_as_commentable_with_threading](https://github.com/elight/acts_as_commentable_with_threading)
2. [auto_html](https://github.com/dejan/auto_html)
3. [bulma-extensions-rails](https://github.com/dhmgroup/bulma-extensions-rails)
4. [bulma-rails](https://github.com/joshuajansen/bulma-rails)
5. [devise](https://github.com/heartcombo/devise)
6. [geocoder](https://github.com/alexreisner/geocoder)
7. [has_friendship](https://github.com/sungwoncho/has_friendship)
8. [link_thumbnailer](https://github.com/gottfrois/link_thumbnailer)
9. [open-weather](https://github.com/coderhs/ruby_open_weather_map)
10. [pagy](https://github.com/ddnexus/pagy)
11. [routes_coverage](https://github.com/Vasfed/routes_coverage)
12. [rubocop](https://github.com/rubocop-hq/rubocop)
13. [rubycritic](https://github.com/whitesmith/rubycritic)

<p align="center">
  <img width="460" height="300" src="https://media.giphy.com/media/kcCkg4ao1PTCQiMkpC/giphy.gif">
</p>

Auto_html, geocoder, has_friendship and link_thumbnailer are the top gems of all gems in Project LiveWire. So if you have an idea, I suggest to check these top gems to inspire you in your next project.

## Models
LiveWire have multiple models, as shown below and each models (application_record is a given) have its purpose to support the active record behavior, and its database. In other words what you really define in the models reflect to the controller's handling of its database. 

```
app/models
├── application_record.rb
├── appointment.rb
├── comment.rb
├── concerns
├── follow.rb
├── location.rb
├── post.rb
├── state.rb
└── user.rb
```



### Relationships
As per project requirement, each model (if necessary) should have at least ```has_many```, ```belongs_to``` and two ```has_many :through``` relationships (or many to many). Below is the ERD model of Project LiveWire, as you can see the users table is the center of all. 

![erd_model.png?raw=true](https://rawcdn.githack.com/johncban/livewire/a33a415b6b8c127a0da1f30c18bdef09867a10dc/app/assets/images/screenshots/erd_model.png?raw=true)


It's a pretty, pretty, pretty... challenging part of the project, making relationships for the model and a few number of irb test just to make sure the dataflow is predictable for the controller.
<p align="center">
  <img width="460" height="300" src="https://media.giphy.com/media/yzYfKupXbnEY0/giphy.gif">
</p>


#### User Model
The User Model have multiple ```has_many``` relationship to support posts, comments, appointments and followers. It also have 2 ```has_many :through``` for comments through posts and appointments through locations.

In line 4 to 6, devise generated modules for authentication, registration and the rest I'll explain it to User Authentication section.
In line 8 to 9, appointments and comments are subjected to has_many :through relationships while 10 to 12 are essential many to many relationships for th user to create appointments, posts and comments.

The user follower feature (line 14 to 17) is pretty much separated because it didn't have schema relationship but it only use the model on how it will behave if the user follow another user under ```many-to-many ``` relationship.

```
class User < ApplicationRecord
  ...
  
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :omniauthable, omniauth_providers: [:google_oauth2]

  has_many :appointments, through: :locations, dependent: :destroy
  has_many :comments, through: :posts
  has_many :appointments, dependent: :destroy
  has_many :posts, dependent: :destroy
  has_many :comments, dependent: :destroy

  has_many :followed_users, foreign_key: :follower_id, class_name: 'Follow', dependent: :destroy
  has_many :followees, through: :followed_users, dependent: :destroy
  has_many :following_users, foreign_key: :followee_id, class_name: 'Follow', dependent: :destroy
  has_many :followers, through: :following_users, dependent: :destroy

  ...

end

```

#### Post Model
Post model is another essential model for the our app to work. In this model, acts_as_commentable gem is initialized and auto_html gem is use for extracting the contents from youtube or a url.
The model ```belongs_to :user``` as associated or reference to the post owner (line 6) and it have many users through comments or 1 ```many-to-many``` relationship (line 9).

```
class Post < ApplicationRecord
  ...

  acts_as_commentable

  belongs_to :user
  delegate :name, to: :user

  has_many :users, through: :comments

  auto_html_for :post_content do
    html_escape
    image
    youtube(width: 260, height: 250, autoplay: false)
    link target: '_blank', rel: 'nofollow'
  end

  ...
  
end

```

#### Comment Model
The Comment Model holds and manages comments from Post Model. It was generated by ```acts_as_commentable_with_threading``` gem to post comments in the user's post and reply to the primary comment that would act as a comment thread.

The said model have ```belongs_to :user``` and it have ```has_many :comments, as: :commentable``` relationship. ```:commentable``` is an alias for ```acts_as_commentable``` that defined from user model for comment threading.

Now noticed ```belongs_to :commentable, polymorphic: true```, its a polymorphic association that can belong to more than one or other model under single association.

```
class Comment < ActiveRecord::Base
  acts_as_nested_set scope: %i[commentable_id commentable_type]
  ...
  
  # NOTE: install the acts_as_votable plugin if you
  # want user to vote on the quality of comments.
  # acts_as_votable

  belongs_to :commentable, polymorphic: true
  has_many :comments, as: :commentable

  # NOTE: Comments belong to a user
  belongs_to :user
  # belongs_to :post --> cannot belongs_to because of acts_as_commentable

  ...

end

```

#### Appointment Model
This model belongs from user model or ```belongs_to :user``` (line 4) with 1 ```has_many``` relationship (line 7) and ```many-to-many``` relationship (line 6).

It also accept nested attributes from locations that appointment model creates. 
```
class Appointment < ApplicationRecord

  ...

  belongs_to :user

  has_many :users, through: :locations
  has_many :locations
  accepts_nested_attributes_for :locations, allow_destroy: true

  ...
  
end
```

#### Location Model

As mentioned before, appointments model have location nested attributes and the location model have such nested attributes that ```belongs_to :user``` and ```belongs_to :appointment``` but here's the cherry on top, the ```actual_location``` method is geocoded for location coordinates to support the MapBox API and COVID-19 API.

```
class Location < ApplicationRecord
  
  ...

  belongs_to :user
  belongs_to :appointment

  geocoded_by :actual_location
  after_validation :geocode

  ...
  

  def actual_location
    [appt_address, appt_city, appt_state].compact.join(', ')
  end
end

```


### Validations
Project LiveWire have a combination of model, view and controller validation. It didn't use ```field_with_errors``` class for views but instead a custom bulma css framework class. LiveWire Project utilize id error validation, such as ```error_explanation``` and a ```form_for``` that catch errors in every form fields (if there's any error).

#### Model Validations
In the model validation, I'll provide a summary of model user validation as all model have validations and there's a lot of material to cover.

##### Application Model
In the Application Model, the validation is strict for 3 fields and it checks if the value is present; otherwise, it will trigger a flash banner and the row will not commit.
```
class Appointment < ApplicationRecord
  validates_presence_of :appt_name, :appt_description, :appt_date

  ...
  
end
```
#### Controller Validations
The Appointment Controller have both authentication and validation as per devise but there's also flash that catch the user's error if the user provide an empty field.

##### Appointment Controller
In this particular controller at line 13 it provide a flash banner if ```@county``` is nil or empty that obtain location information from line 9 to provide county location to a method called ```covidCountyData```. 

```
class AppointmentsController < ApplicationController
  before_action :authenticate_user!
  
  ...

  def show
    find_appointment
    if @appointment.user_id == current_user.id
      address_results = Geocoder.search([@appointment.locations.first.latitude,   @appointment.locations.first.longitude])
      @county = address_results.first.county
      @state = @appointment.locations.first.appt_state
      if @county.nil?
        flash[:warning] = "Covid 19 Data at #{@state} Boroughs Are Not Available!"
      else
        covidCountyData
      end
    else
      flash[:error] = 'Record Access is Restricted.'
      redirect_to user_appointments_path
    end
  end

  ...

  
end

```

#### View Validations
Under view validations, most of the form utilize ```form_for``` and bulma's notification class.

##### Appointment Form Fields
From line 3 to 63 it scans the form fields if its invalid or empty, if so line 4 will be triggered then notify the user which form field needs to be validated.
```
<div class="container is-three-quarters">
  <section class="apt-shadow">
    <%= form_for [@user, @appointment], url: user_appointments_path do |form| %> <%# https://stackoverflow.com/questions/41147681/undefined-method-appointments-path-using-nested-resource %>
      <% if appointment.errors.any? %>
        <div id="error_explanation">
          <div class="notification is-warning">
            <button class="delete"></button>
            <h2><%= pluralize(appointment.errors.count, "error") %> Prohibited this appointment from being saved:</h2>
            <ul>
              <% appointment.errors.full_messages.each do |message| %>
                <li><%= message %></li>
              <% end %>
            </ul>
          </div>
        </div>
      <% end %>
    <br>
      <div class="field">
        <label class="label">Appointment</label>
          <div class="control">
            <%= form.text_field :appt_name, class: 'input', placeholder: "Appt Name" %>
          </div>
      </div>

      <div class="field">
        <label class="label">Description</label>
          <div class="control">
            <%= form.text_area :appt_description, class: "textarea", placeholder: "Appt Description" %>
          </div>
      </div>

      <div class="field">
        <label class="label">Appointment Date</label>
        <%= form.datetime_select :appt_date, class: "select" %>
      </div>

      <%= form.fields_for :locations do |loc| %>
        <div class="field">
        <label class="label">Appointment Address</label>
          <div class="control">
            <%= loc.text_field :appt_address, class: 'input', placeholder: "Appt Address" %>
          </div>
        </div>

        <div class="field">
          <label class="label">Appointment City</label>
            <div class="control">
              <%= loc.text_field :appt_city, class: 'input', placeholder: "Appt City" %>
            </div>
        </div>

        <div class="field">
          <label class="label">State</label>
          <%= loc.collection_select :appt_state, State.all, :statename, :statename, include_blank: true %>
        </div>
      <% end %>

    <hr>
    <div class="columns is-12">
        <div class="column">
          <%= form.submit 'Save', :class => 'button is-primary' %>
        </div>
    <% end %>
      <div class="column is-12">
         <%= button_to "Back", user_appointments_path, :class => "button is-primary is-outlined", method: :get%>
      </div>
    </div>
    <hr>
  </section>
</div>

```


### Scope
Project LiveWire have 2 scopes for at least 2 models, the most common is Post Model that will show in this section.

In Post Model, line 4 and 6 the scope are interconnected to ```post_controller``` if the post need to publish for public view or private using boolean.
```
class Post < ApplicationRecord
  ...

  scope :published, -> { where(published: true) }
  scope :unpublished, -> { where(published: false) }

  ...
end

```
Under Comment Model, the default scope is ordered to descending as per created date of the comment. Hence, it show the users comments from the latest date to late or early posted comments.
```
class Comment < ActiveRecord::Base
  
  ...
  
  default_scope -> { order('created_at DESC') }
  
  ...
  
end

```
In Posts Controller, the posts are reorder to descending just like the comment model but the unique part its utilize by Pagy gem (line 15).

```
class PostsController < ApplicationController
 
  ...

  include Pagy::Backend

  ...

  def index
    @posts = if params[:user_id] == current_user.id
               User.find(params[:id]).posts
             else
               Post.published
             end
    @pagy, @posts = pagy(@posts.reorder('created_at DESC'), page: params[:page], items: 4)
  end

  ...

end

```


## User Authentication
Project LiveWire have devise for user authentication and devise mostly use for MVC solution based in Rails.
For step-by-step details, I suggest to read my [Devise blog](https://johncban.github.io/devise_for_my_first_ror_project).

### Devise
As explained from my [blog](https://johncban.github.io/devise_for_my_first_ror_project), Devise is rack based, modular and allows multiple models to sign in at the same time! It means it will secure your app and provide an ease deploying Omniauth for third party authentication. 

### Google OAuth
Project LiveWire have Google OAuth as Devise's Omniauth, its deployed in the user model in line 7 and line 11 to 20.
The ```self.from_omniauth(auth)``` method handle Google's Oauth for gmail email, password and name (optional).
Also, the setup will require ```gem 'omniauth-google-oauth2' ``` bundle installation and if you wanted brief information for set-up I suggest to take a look in this [repo strategy](https://github.com/zquestz/omniauth-google-oauth2).

```
class User < ApplicationRecord

  ...

  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :omniauthable, omniauth_providers: [:google_oauth2]
  
  ...
  
  def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0, 20]
      user.name = auth.info.name # assuming the user model has a name
      # If you are using confirmable and the provider(s) you use validate emails,
      # uncomment the line below to skip the confirmation emails.
      # user.skip_confirmation!
    end
  end
end

```
## REST
The routes or resource routes from Project LiveWire are nested but devise have its own routes and authenticated users go straight to user dashboard if verified via devise.

The following is the summary of routes configuration.

### Nested Resource
Posts and Appointment routes are nested under users (parent resource) that would allow the user to show, edit and update posts, and appointments. Also, follow user button is present for users; however, friends is isolated as per gem support and there's a match route to find routing error then reforward the user in the dashboard if the route is invalid.
```
Rails.application.routes.draw do
  devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }

  authenticated :user do
    root 'user_dashboard#index', as: :authenticated_root
  end

  devise_scope :user do
    get '/users', to: 'devise/registrations#new'
    get '/users/password', to: 'devise/passwords#new'
  end

  resources :users, only: %i[show edit update] do
    post :'/users/:id/follow', to: 'users#follow', as: 'follow_user'
    post :'/users/:id/unfollow', to: 'users#unfollow', as: 'unfollow_user'
    post :'/button', to: 'posts#test_a_button', as: 'button'
    resources :posts
    resources :appointments
  end

  resources :appointments, only: [:show]
  resources :comments
  resources :friends, only: %i[index create]

  post '/friends/add' => 'friends/add'
  post '/friends/reject' => 'friends/reject'
  post '/friends/remove' => 'friends/remove'
  get '/friends/search' => 'friends/search'
  post '/friends/search' => 'friends/search'

  # For details on the DSL available within this file, see https://guides.rubyonrails.org/routing.html
  root 'main#main'

  match '*path', to: 'application#routing_error', via: %i[get post]

  mount Localtower::Engine, at: 'localtower' if Rails.env.development? && defined?(Localtower)
end

```

## DRY - Do Not Repeat Your Self 
Project LiveWire's model is properly encapsulated (if applicable) so it will support has_friendship gem and auto_html.
It's not only the models that are encapsulated but also the Controller and Views (using controller helper).

### Controller
In the controller side, ```users_controller```, ```user_dashboard_controller```, ```posts_controller```, ```comments_controller``` and ```appointment_controller``` have a reasonable DRY pattern as repetitive objects are turned into methods.

The User Dashboard Controller is very dependent to APIs to provide live information, such as geographical IP to get user's location, live news from the user's country and the user's weather locaiton.

From a sphagetti code, I manage to encapsulate the APIs into methods that index will call once the dashboard loads. In addition, I can reuse the API methods (line 12 to 14) for future use.
```
require 'open_weather'

class UserDashboardController < ApplicationController
  before_action :authenticate_user!

  def index
    @user = User.all
    @post = Post.all
    @appointment = Appointment.all
    @news_key = ENV['NEWS_API_KEY']

    getIP
    getWeather
    getNews
  end
  
  ...
  
  private

  def getIP
    url = URI('https://freegeoip.app/json/')

    http = Net::HTTP.new(url.host, url.port)
    http.use_ssl = true
    http.verify_mode = OpenSSL::SSL::VERIFY_NONE

    request = Net::HTTP::Get.new(url)
    request['accept'] = 'application/json'
    request['content-type'] = 'application/json'

    response = http.request(request)
    @ipdata = JSON.parse(response.body)
  end

  def getWeather
    lat = @ipdata['latitude']
    lon = @ipdata['longitude']

    options = { units: 'imperial', APPID: ENV['OWM_API_KEY'] }
    weather_info = OpenWeather::Current.geocode(lat, lon, options)

    dat = weather_info.to_json
    @current_w = JSON.parse(dat)
  end

  def getNews
    @country = @ipdata['country_code']

    url = "https://newsapi.org/v2/top-headlines?country=#{@country}&sortBy=publishedAt&apiKey=#{@news_key}"

    resp = Net::HTTP.get_response(URI.parse(url))
    @data = JSON.parse(resp.body)
    @news_count = @data['articles'].size
  end
end

```

### View Helper
User Dashboard Helper contains helper methods for views and it makes the user dashboard more DRY.
```
module UserDashboardHelper
  def dashboard_id
    @dashboard_id ||= current_user.id
  end

  def dashboard_name
    @dashboard_name ||= current_user.name
  end

  def followees_count
    @followees_count = current_user.followees.count
  end

  def followers_count
    @followers_count = current_user.followers.count
  end

  def appointment_count
    @appointment_count = current_user.appointments.count
  end
end

```
Below ```dashboard_id```, ```dashboard_name```, ```followees_count```, ```followers_count``` and ```appointment_count``` was defined as methods from ```UserDashboardHelper```
```
<section class="hero is-medium is-light is-bold hero-bkg-glass">
  <div class="hero-body">
    <div class="container">
      <h1 class="title">
        LiveWire Dashboard
      </h1>
        <div class="shadow user-name-bkg-glass">
          <h2 class="title user-name-margin">
            <a href="/users/<%= dashboard_id %>">
              <%= dashboard_name %>
            </a>
          </h2>
        </div>
        <br>
      <h4>
         <div class="columns is-mobile is-multiline">
                  <div class="column is-narrow">
                        <p class="bd-notification is-primary">
                            <%= dashboard_name %> follow <%= followees_count %> users.
                            <br>
                            <%= dashboard_name %> have <%= followers_count %> followers.
                        </p>
                  </div>
          </div>
      </h4>
    </div>
  </div>
</section>

<br>

<div class="tile is-ancestor">
  <div class="tile is-vertical is-8">
    <div class="tile">
      <div class="tile is-parent is-vertical">
        <article class="tile is-child notification is-primary shadow">
          <h1 class="title">Number of Appointments</h1>
          <p class="subtitle is-size-1"><%= appointment_count %></p>
        </article>
        <article class="tile is-child notification is-dark shadow">
          <p class="title">Number of Registered Users</p>
          <p class="subtitle is-size-1"><%= @user.count %></p>
        </article>
      </div>

      <div class="tile is-parent">
        <article class="tile is-child notification is-black shadow">
          <p class="title">News Feed</p>
          <p class="subtitle">
            There are <%= @news_count%>  Articles Available
          </p>
         <div class="news-scroll-content block">
            <% @data["articles"].each do |articles| %>
              <ul>
                        <li>
                            <a href="<%= articles['url'] %>" target="_blank" rel="noopener noreferrer" class="no-underline">
                              <!-- https://www.thesitewizard.com/html-tutorial/open-links-in-new-window-or-tab.shtml -->
                              <h2>
                                  <%= articles['title'] %>
                              </h2>
                                <small>
                                  <%= articles['publishedAt'] %>
                                </small>
                                <figure class="image is-4by3">
                                  <img src="<%= articles['urlToImage'] %>">
                                </figure>
                            </a>
                            <br><br>
                        </li>
              </ul>
            <% end %>
          </div>
        </article>
      </div>
    </div>
    <div class="tile is-parent">
      <article class="tile is-child notification is-black shadow">
        <p class="title">Your Location</p>
        <p class="subtitle">
          <%= @ipdata['city'] %>, <%= @ipdata['region_code'] %> Temperature is <%= @current_w['main']['temp'] %> &deg;F but Feel's Like <%= @current_w['main']['feels_like'] %> &deg;F
        </p>
      </article>
    </div>
  </div>
  
  <div class="tile is-parent">
    <article class="tile is-child notification is-primary shadow">
      <div class="content column is-centered">
        <p class="title">Posts</p>
        <p class="subtitle">
          There are <%= current_user.posts.count %> Posts | 
          <%= link_to '<i class="fas fa-file-alt"></i> Create New Post'.html_safe, new_user_post_path(@user, @post) %>
        </p>
        <div class="posts-scroll-content column has-text-centered is-14">
          <!-- Content --> 
         
            <%= render "posts"%>
            
        </div>
      </div>
    </article>
  </div>
  
</div>

<%= render "layouts/footer" %>
```
## Ruby Critic
I also suggest to use [RubyCritic](https://github.com/whitesmith/rubycritic), it's a gem that analyze your code's quality and provide you an overall grade. 
So far, project LiveWire still have a place for progress.

![rubyCritic.png](https://rawcdn.githack.com/johncban/livewire/1c4e876de184ac42fa1bfb8272c639a026e10e1a/app/assets/images/screenshots/rubyCritic.png)

