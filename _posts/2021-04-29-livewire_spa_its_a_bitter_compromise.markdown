---
layout: post
title:      "LiveWire SPA, its a bitter compromise"
date:       2021-04-29 08:03:03 +0000
permalink:  livewire_spa_its_a_bitter_compromise
---

> This is my 4th Flatiron Project and its a tough one from the rest of the project I previously encountered...

I know the first quote is blunt but it meant to be, I tried to make the app work since October last year but unfortunately I am bitterly out of time and this would be my last here in Flatiron.
This particular project ate a lot of time because of my pride but I should instead compromise and follow this [setup](https://github.com/learn-co-curriculum/rails-js-project-setup) but instead I read and follow vanilla js books and attended open sessions despite of my new job's difficult schedule.

But enough regrets! what is done is done and I have to explain how I implement this compromising project.



## Model
I start planning my model using my previous project since Sinatra, I started creating User, Portfolio and Stock as my models.

<br>
![](https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/erd.png)
<br>

```
 create_table "portfolios", force: :cascade do |t|
    t.string "portfolio_name"
    t.integer "user_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["user_id"], name: "index_portfolios_on_user_id"
  end

  create_table "stocks", force: :cascade do |t|
    t.string "stock_name"
    t.integer "quantity"
    t.integer "portfolio_id", null: false
    t.integer "user_id", null: false
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
    t.index ["portfolio_id"], name: "index_stocks_on_portfolio_id"
    t.index ["user_id"], name: "index_stocks_on_user_id"
  end

  create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "email"
    t.string "password_digest"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  add_foreign_key "portfolios", "users"
  add_foreign_key "stocks", "portfolios"
  add_foreign_key "stocks", "users"
```

### User Model
This particular model is simple as vanilla, a typical username, email and plain ol' bcrypt; however, don't get fooled by the backend jwt will support the session to secure the user which I'll explain later. 

```
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string :username
      t.string :email
      t.string :password_digest
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string :username
      t.string :email
      t.string :password_digest

      t.timestamps
    end
  end
end
      t.timestamps
    end
  end
end
```

### Portfolio Model
This model meant to attach relationship between users and stocks. It's simple as `abc` but it helps the livewire app manage the user's stock or stocks to screen.

```
class CreatePortfolios < ActiveRecord::Migration[6.0]
  def change
    create_table :portfolios do |t|
      t.string :portfolio_name

      
      t.belongs_to :user, null: false, foreign_key: true
      
      t.timestamps
    end
  end
end
```

### Stocks Model
The Stocks Model is the center stage of this app, its the user's assets that meant to be screened and related to its respective portfolio. It's referenced to Portfolio but belong to User and it have only two columns, `stock_name` and `quantity`. The reason why I didn't put too much data is for speed, portability and simplicity. Let the API do all the work as Market Data always change in a microsecond and some info (or mostly) didn't need to be stored. 

```
class CreateStocks < ActiveRecord::Migration[6.0]
  def change
    create_table :stocks do |t|
      t.string :stock_name
      t.integer :quantity
      t.references :portfolio, null: false, foreign_key: true
      t.belongs_to :user, null: false, foreign_key: true
      t.timestamps
    end
  end
end
```
<br>
<br>
<img src="https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/pexels.jpg" alt="Image by Pexels.com" width="800"/>
<br>
<br>
<br>
## Controller
On this part, I'll explain the creme a la creme and back bone of this project. The SPA's controller is unique because its generated through API.
```
rails new livewire_api --api
```
It's surprisingly interesting too because you can do versioning if you decided to upgrade the controller.
Before I proceed, I am excited to show my Gems I used in this project as it follows:
<br>
```
Gems included by the bundle:
  * actioncable (6.0.3.6)
  * actionmailbox (6.0.3.6)
  * actionmailer (6.0.3.6)
  * actionpack (6.0.3.6)
  * actiontext (6.0.3.6)
  * actionview (6.0.3.6)
  * active_model_serializers (0.10.12)
  * activejob (6.0.3.6)
  * activemodel (6.0.3.6)
  * activerecord (6.0.3.6)
  * activestorage (6.0.3.6)
  * activesupport (6.0.3.6)
  * bcrypt (3.1.16)
  * bootsnap (1.7.4)
  * builder (3.2.4)
  * byebug (11.1.3)
  * case_transform (0.2)
  * concurrent-ruby (1.1.8)
  * crass (1.0.6)
  * erubi (1.10.0)
  * ffi (1.15.0)
  * globalid (0.4.2)
  * i18n (1.8.10)
  * jsonapi-renderer (0.2.2)
  * jwt (1.5.6)
  * knock (2.1.1)
  * listen (3.5.1)
  * loofah (2.9.1)
  * mail (2.7.1)
  * marcel (1.0.1)
  * method_source (1.0.0)
  * mini_mime (1.1.0)
  * minitest (5.14.4)
  * msgpack (1.4.2)
  * nio4r (2.5.7)
  * nokogiri (1.11.3)
  * puma (4.3.7)
  * racc (1.5.2)
  * rack (2.2.3)
  * rack-cors (1.1.1)
  * rack-test (1.1.0)
  * rails (6.0.3.6)
  * rails-dom-testing (2.0.3)
  * rails-html-sanitizer (1.3.0)
  * railties (6.0.3.6)
  * rake (13.0.3)
  * rb-fsevent (0.10.4)
  * rb-inotify (0.10.1)
  * spring (2.1.1)
  * spring-watcher-listen (2.0.1)
  * sprockets (4.0.2)
  * sprockets-rails (3.2.2)
  * sqlite3 (1.4.2)
  * thor (1.1.0)
  * thread_safe (0.3.6)
  * tzinfo (1.2.9)
  * websocket-driver (0.7.3)
  * websocket-extensions (0.1.5)
  * zeitwerk (2.4.2)
```
<br>
It seems a lot from my bundle but the top gems I use is knock and jwt.
<br>
#### Knock
[Knock](https://github.com/nsarno/knock), it utilize jwt for authentication and it secures the user's session everytime they login.
It also secure the user model and most of all it uses jwt! 
You can store jwt token either in local storage or session storage ([I know its a bad practice!](https://dev.to/gkoniaris/how-to-securely-store-jwt-tokens-51cf) but I am desperate!)

### Application Controller, Users Controller, Portfolios Controller and Users Controller
Alright, I combined it in one line on this section its just too much to cover but I'll make it sum and sweet.
In Application Controller, it handle the `include Knock::Authenticable` that handle user session for the rest of the models. 
The Users Controller, have index, show, create and update also delete (due to time constraints I can't pursue profile deletion and update... sigh...). 
Then the Portfolios Controller, it have all CRUD capability such as index, show, create destroy and update.
The Stock Controller have index, show, create and destroy (unfortunately updating a stock is tough to implement in javascript).
All controllers render JSON and serialized to support the frontend.

#### Routes 
I know, I know why do I include routes, its actually an important part for the View area and the requirements for this project. Anyway I'll just have to show it as I put my time and pretty much sleepless tears to make it work.
<br>
```
Rails.application.routes.draw do
  
  scope '/auth' do
    post '/signin', to: 'user_token#create'
    post '/signup', to: 'users#create'
  end


  namespace :api, defaults: { format: 'json' } do
    namespace :v1 do
      resources :users, only: [:index, :create, :destroy, :update, :show] do 
        resources :portfolios, only: [:index, :create, :destroy, :update, :show] do
          resources :stocks, only: [:index, :create, :destroy, :update, :show]
        end
      end
    end
  end


end
```
<br>
Trying to make the routes and its resources properly acessible is the key for successful frontend CRUD that is why I put effort as much as possible for javascript fetch.

## Views 
![vjs](https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/vjs.png)
<br>
Not its time for the views, LiveWire-SPA users Vanilla JS Ecma Script version 5~6.
So here's the front-end directory tree:
```
.
├── index.html
├── src
│   ├── adapters
│   │   ├── portfoliosAdapter.js
│   │   └── usersAdapter.js
│   ├── components
│   │   ├── app.js
│   │   ├── libs
│   │   │   ├── background.js
│   │   │   ├── skycons.js
│   │   │   └── weather.js
│   │   ├── portfolio
│   │   │   ├── portfolio.js
│   │   │   └── portfolios.js
│   │   ├── stock
│   │   │   └── stock.js
│   │   └── user
│   │       ├── localprofile.js
│   │       ├── usersigninform.js
│   │       └── usersignupform.js
│   └── index.js
└── styles
    └── style.css

8 directories, 15 files
```
To match with the requirement it should be "One" or "Single" hence its SPA or Single Page Application.
However, in scheme of the front-end project there are only few essential parts such as the adapters, components, portfolio, stock and user then index that reflect with the model and controller.
The adapter do the work to fetch the serialized data through routes.
```
...
 loginUsers = (loginData) => {
        console.log({
            auth: loginData
        })
        const config = {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
                Accept: "application/json",
            },
            body: JSON.stringify({
                auth: loginData
            }),
        };

        return fetch(`${this.baseURL}/auth/signin`, config)
            .then((res) => {
                if (res.status >= 200 && res.status <= 299) {
                    return res.json()
                        .then(json => {
                            sessionStorage.setItem('jwt', json.jwt)
                            console.log('Token:', sessionStorage.getItem('jwt'))
                            this.renderUserProfile()
                            console.log(json)
                        })
                } else {
                    console.log(res.status, res.statusText);
                    alert(`User -- ${res.statusText}`)
                }
            })
    }
...
```

## API 
Now for the special mention, the APIs that are utilized are reliable (and somewhat expensive). 
<br>
### Weather - Dark Sky
![](https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/darksky.png)
<br>
### Stock Data - IEX Cloud
![](https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/iex.png)
<br>
### Background Effects - Vanta.js 
<img src="https://raw.githubusercontent.com/johncban/livewire_spa/main/IMAGES/vanta_js.png" alt="Image by Pexels.com" width="400"/>










