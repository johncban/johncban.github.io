---
layout: post
title:      "Sinatra + Stocks = Stocky"
date:       2020-04-18 15:01:45 -0400
permalink:  sinatra_stocks_stocky
---


Its been several months since October and back November of last year I started learning Sinatra. It's a long jazzy journey, but just like its famous song I do it "My Way".

![Sinatra](https://media.giphy.com/media/sI1XAGwxRYIBa/giphy.gif)

So what is Sinatra, its not about jazz or the Rat Pack or the world famous NJ native classic jazz singer. Sinatra is a lightweight version of Ruby on Rails with minimal stack requirements for simple dynmic Ruby web applications development.

It's so simple and lightweight that it requires you (or myself) as a developer to create a bundle then prepare the Gemfile for sinatra and other required gems for my project. In addition, Sinatra library is based on DSL or Domain Specific Language that access HTTP protocol request easier (Remember GET and POST ?).

Now, without said I begin combining sinatra and stocks to create Stocky.
 

[<img width="420" height="250" src="https://media.giphy.com/media/B8NSo2tYq4SRi/giphy.gif">](http://sinatrarb.com/)

[<img width="420" height="250" src="https://media.giphy.com/media/3Y6Vtk1QrTKU/giphy.gif">](https://rapidapi.com/blog/best-stock-api/)

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; 

## Corneal and APIs
Project planning is the ironic challenge in the beginning a project because it requires research, testing and prototyping.
It took me almost 2 weeks, researching for API, gem library testing and  building Sinatra app with the integrated API and gems.
Truth be told its challenging especially if most of the APIs and gems have limitations for my project's requirement.

However, things got easier for generating Sinatra app and testing all thanks to Corneal.

[<img align="left" width="auto" height="250" src="https://thebrianemory.github.io/corneal/images/corneal-medium.png">](https://thebrianemory.github.io/corneal/)

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; 
&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; 
&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; 

A gem called Corneal let's me generate a Sinatra template with ease starting by installing a local gem to my client environment.

```
gem install corneal
```

Then I start creating my app.

```
corneal sinatra-stocky
```

Afterwards, it will generate the following files and directory.

```
.
└── sinatra-stocky
    ├── Gemfile
    ├── README.md
    ├── Rakefile
    ├── app
    │   ├── controllers
    │   │   └── application_controller.rb
    │   ├── models
    │   └── views
    │       ├── layout.erb
    │       └── welcome.erb
    ├── config
    │   ├── environment.rb
    │   └── initializers
    ├── config.ru
    ├── db
    │   └── migrate
    ├── lib
    ├── public
    │   ├── favicon.ico
    │   ├── images
    │   │   └── corneal-small.png
    │   ├── javascripts
    │   └── stylesheets
    │       └── main.css
    └── spec
        ├── application_controller_spec.rb
        └── spec_helper.rb
```

Inside the project folder or sinatra-stocky folder, the setup begins by using ```bundle``` but before using the said command it will require to alter the Gemfile contents from 
```
gem 'database_cleaner', git: 'https://github.com/bmabey/database_cleaner.git'
```
to 
```
gem 'database_cleaner'
```

Once the app runs from using ```shotgun```, it will show only a pure index file template. 
&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 
This project will require Model View Control or MVC, Active Record, user account, multiple models and model relationship. Corneal can generate model, controller and associate MVC! 

In this project, I have three models and by using Corneal it will be the following process
```
corneal model user
corneal model portfolio
corneal model stock
```
It will generate the following files from model directory along with my migratation files from migrate folder under db.
```
.
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   │   ├── portfolio.rb
│   │   ├── stock.rb
│   │   └── user.rb
│   └── views
│       ├── layout.erb
│       └── welcome.erb
├── config
│   ├── environment.rb
│   └── initializers
├── config.ru
├── db
│   ├── development.sqlite
│   └── migrate
│       ├── 20200414183825_create_users.rb
│       ├── 20200414183856_create_portfolios.rb
│       └── 20200414183902_create_stocks.rb
├── lib
├── public
│   ├── favicon.ico
│   ├── images
│   │   └── corneal-small.png
│   ├── javascripts
│   └── stylesheets
│       └── main.css
└── spec
    ├── application_controller_spec.rb
    └── spec_helper.rb
```
Corneal can also define the attributes of the project's model
I also have added three controllers to support application_controller:
1. portfolio_controller.rb
2. stock_controller.rb
3. user_controller.rb

In Corneal, by just typing the following commands it will generate the above controller files for you.
```
corneal controller user
corneal controller portfolio
corneal controller stock
```
After providing the above's command, it will generate not only the controller but also the views for the project.
```
.
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── app
│   ├── controllers
│   │   ├── application_controller.rb
│   │   ├── portfolios_controller.rb
│   │   ├── stocks_controller.rb
│   │   └── users_controller.rb
│   ├── models
│   │   ├── portfolio.rb
│   │   ├── stock.rb
│   │   └── user.rb
│   └── views
│       ├── layout.erb
│       ├── portfolios
│       │   ├── edit.html.erb
│       │   ├── index.html.erb
│       │   ├── new.html.erb
│       │   └── show.html.erb
│       ├── stocks
│       │   ├── edit.html.erb
│       │   ├── index.html.erb
│       │   ├── new.html.erb
│       │   └── show.html.erb
│       ├── users
│       │   ├── edit.html.erb
│       │   ├── index.html.erb
│       │   ├── new.html.erb
│       │   └── show.html.erb
│       └── welcome.erb
├── config
│   ├── environment.rb
│   └── initializers
├── config.ru
├── db
│   ├── development.sqlite
│   └── migrate
│       ├── 20200414183825_create_users.rb
│       ├── 20200414183856_create_portfolios.rb
│       └── 20200414183902_create_stocks.rb
├── lib
├── public
│   ├── favicon.ico
│   ├── images
│   │   └── corneal-small.png
│   ├── javascripts
│   └── stylesheets
│       └── main.css
└── spec
    ├── application_controller_spec.rb
    └── spec_helper.rb
```
Corneal provides ease creating Sinatra apps and provides developers an idea or big picture how to create a direction for the project. Speaking of direction, next would be the API deployment using the following key API to make Sinatra Stocky possible.

&nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp;

#### IEX Ruby Client

IEX is obviously an API platform that provides finanacial data available to everyone. It's based in REST and returns HTTP response codes.

[<img width="500" height="auto" src="https://mms.businesswire.com/media/20190522005193/en/723157/23/logo-color.jpg">](https://iexcloud.io/)

There's a gem called [IEX-Ruby-Client](https://github.com/dblock/iex-ruby-client#get-an-api-token), it requires an API token from IEX Cloud; however, it provides full compatability with Ruby and Sinatra.

To install IEX-Ruby-Client, just add the following gem in the Gemfile.
```
gem 'iex-ruby-client'
```

#### Alpha Vantage 

Alphavantage is another free API stocks and financial platform. It have a free API token but the free version have limitations in stock symbol search and stock history data.

[<img width="500" height="auto" src="https://apifriends.com/wp-content/uploads/2018/02/alpha-vantage-home.png">](https://www.alphavantage.co/)

AlphavantageRB is a gem that's fully compatible with Ruby and Sinatra, to install the said gem it requires the following gem repository to the Gemfile.

```
gem 'alphavantagerb'
```

## Sinatra-Stocky Project Summary

After further refractoring the results from provided from Corneal and adding additional gems to the project's Gemfile, here are the results.
```
.
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── app
│   ├── controllers
│   │   ├── application_controller.rb
│   │   ├── portfolio_controller.rb
│   │   ├── stock_controller.rb
│   │   └── user_controller.rb
│   ├── models
│   │   ├── portfolio.rb
│   │   ├── stock.rb
│   │   └── user.rb
│   └── views
│       ├── admin
│       │   └── allusers.erb
│       ├── failure.erb
│       ├── home.erb
│       ├── index.erb
│       ├── layout.erb
│       ├── portfolio
│       │   ├── create_portfolio.erb
│       │   ├── edit_portfolio.erb
│       │   ├── show_portfolio.erb
│       │   └── user_portfolio.erb
│       ├── stocks
│       │   ├── new_stock.erb
│       │   ├── stock.erb
│       │   ├── stock_edit.erb
│       │   └── user_stock_info.erb
│       └── users
│           ├── edit.erb
│           ├── login.erb
│           ├── profile.erb
│           └── signup.erb
├── config
│   ├── environment.rb
│   └── initializers
├── config.ru
├── db
│   ├── development.sqlite
│   ├── migrate
│   │   ├── 20191223030736_create_users.rb
│   │   ├── 20200103093559_create_portfolio.rb
│   │   └── 20200103093820_create_stocks.rb
│   ├── schema.rb
│   └── test.sqlite
├── doc
│   ├── ApplicationController.html
│   ├── Portfolio.html
│   ├── PortfolioController.html
│   ├── Stock.html
│   ├── StockController.html
│   ├── Stocks.html
│   ├── User.html
│   ├── UserController.html
│   ├── _index.html
│   ├── class_list.html
│   ├── css
│   │   ├── common.css
│   │   ├── full_list.css
│   │   └── style.css
│   ├── debug.log
│   ├── file.README.html
│   ├── file_list.html
│   ├── frames.html
│   ├── index.html
│   ├── js
│   │   ├── app.js
│   │   ├── full_list.js
│   │   └── jquery.js
│   ├── method_list.html
│   └── top-level-namespace.html
├── lib
├── public
│   ├── images
│   │   ├── corneal-small.png
│   │   ├── favicon.ico
│   │   ├── stocky_logo.jpg
│   │   └── stocky_logo.png
│   ├── javascripts
│   │   ├── Chart.bundle.js
│   │   ├── chartkick.js
│   │   └── util.js
│   └── stylesheets
│       └── main.css
├── spec
│   ├── application_controller_spec.rb
│   └── spec_helper.rb
└── spec.md
```

### Model

Sinatra-Stocky uses Active Record (sqlite) as its Model from MVC and the project's Model plan are composed of Users, Portfolio and Stocks.

<img src="https://raw.githubusercontent.com/johncban/sinatra-stocky/master/ScreenShot/Screen%20Shot%202020-04-14%20at%204.40.39%20PM.png" width="40%">


#### User Model

The user model has_many or one-to-many association to portfolios and stocks but at the same time it can be deleted (or destroyed) along with the depending portfolio.
The said model has secured password for validation purposes on the username, email and most of all password. While its uniqueness is validated through username id as explain in the following model ```user.rb```.

```
class User < ActiveRecord::Base
    has_secure_password
    
    validates_presence_of :username, :email, :password, uniqueness: true
    validates_uniqueness_of :username, scope: :id
    validates :email, email: true

    has_many :portfolios, :dependent => :destroy
    has_many :stocks
end

```

#### Portfolio Model

The portfolio model, belongs_to (or one-to-one model) the user model at the smae time it have one-to-many association to stocks model and it can delete the all (```:dependent => :destroy```) the stocks associated to the portfolio model.

```
class Portfolio < ActiveRecord::Base

    belongs_to :user
    has_many :stocks, :dependent => :destroy

end
```

#### Stock Model

The stock model is the leaf of the model that belongs_to or associated as one-to-one model to user model and portfolio model.

```
class Stock < ActiveRecord::Base 
    belongs_to :user 
    belongs_to :portfolio
end
```

### Views and Bootstrap 4

Sinatra-Stocky view have multiple pages as implied in the directory tree and the layout page view supports the rest of the view page for CSS and JS relative path to avoid repetition in each view pages. Also, the ```<%= yield %>``` block pass the method call from other views as the layout page act as a template for the rest of the views.

```
layout.erb

<!DOCTYPE html>
<!--[if lt IE 7] <html class="no-js ie6 oldie" lang="en"-->
<!--[if IE 7] <html class="no-js ie7 oldie" lang="en"-->
<!--[if IE 8] <html class="no-js ie8 oldie" lang="en"-->
<!--[if gt IE 8]><!-->
<html class="no-js" lang="en">
<!--<![endif]-->
    <head>

        <meta charset="utf-8" />
        <meta content="IE=edge, chrome=1" http-equiv="X-UA-Compatible" />
        
        <title>
            Stocky
        </title>
        
        <script src="https://www.gstatic.com/charts/loader.js"></script>
        <script src="https://www.google.com/jsapi"></script>
        <script src="/javascripts/chartkick.js"></script>
        <script src="/javascripts/util.js"></script>
        
        <meta content="width=device-width, initial-scale=1.0" name="viewport"/>
        <link href="/stylesheets/main.css" rel="stylesheet"/>
        <link rel="icon" href="/images/favicon.ico"/>

        <!-- Bootstrap CSS -->
        <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous"> 
        <!-- Material Design CSS -->
        <link rel="stylesheet" href="https://fonts.googleapis.com/icon?family=Material+Icons">

    </head>

    <body>
        <img src="https://download.hipwallpaper.com/desktop/1920/1080/43/9/HcIoBN.jpg" class="img-responsive main-image">

        
            <% flash.keys.each do |type| %>
                <div data-alert class="flash alert alert-danger text-danger rounded-0" role="alert">
                    <%= flash[type] %>
                </div>
            <% end %>

           
        <div id="container container-flex">
          <%= yield %>
        </div>       
    
 
    <footer class="branding frost-footer text-danger">
        <small>
            © 2019<strong> STOCKY</strong>
        </small>
    </footer>
    
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js">
    </script><!--[if lt IE 7]

    | <script src="//ajax.googleapis.com/ajax/libs/chrome-frame/1.0.2/CFInstall.min.js"></script

    | <script>window.attachEvent("onload",function(){CFInstall.check({mode:"overlay"})})</script-->

    <!-- Optional JavaScript -->
        <!-- jQuery first, then Popper.js, then Bootstrap JS -->
        <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js" integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js" integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo" crossorigin="anonymous"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>


    </body>
</html>
```

The project's CSS framework is Bootstrap, its the most common framwork for front end developers and the most convenient framework to use especially creating a modal for single page views.




### Controllers and API integration

Sinatra-Stocky have the following four controllers.
1. application_controller
2. user_controller
3. portfolio_controller
4. stock_controller

There are only 2 controllers that have APIs, and these controllers are the essential to provide API financial information.
These 2 controllers are user_controller and stock_controller.

#### User Controller

The User Controller provides request to CRUD user profile, but it also contains API form Alpha Vantage to generate the user's home dashboard.

<img src="https://raw.githubusercontent.com/johncban/sinatra-stocky/master/ScreenShot/Screen%20Shot%202020-04-16%20at%206.20.20%20AM.png" width="90%">


The dashboard provide reatime historical sector performance and sector five day performance from S&P 500. As mentioned before, the API requires token in order to fect the Alpha Vantage financial information.

```
require 'pry'


class UserController < ApplicationController

	token = ""

	
	...

	
	get '/home' do
		if logged_in?
			@user = User.find(session[:user_id])

			sector = Alphavantage::Sector.new key: token 

			@relTime = sector.real_time_performance
			@fiveDay = sector.five_day_performance
			
			erb :home
		else
			not_log_redirect
		end
	end
	
    ...


end
```

#### Stock Controller

The Stock Controller have the important information from the API mentioned earlier, in this said controller it helps the user to screen research a stock symbol before saving the stock and again screen research a saved stock symbol before patching the current stock record.

<img src="https://github.com/johncban/sinatra-stocky/blob/master/ScreenShot/Screen%20Shot%202020-04-16%20at%206.37.41%20AM.png?raw=true" width="80%">

*Fig. 1: Stock Search*


<img src="https://github.com/johncban/sinatra-stocky/blob/master/ScreenShot/Screen%20Shot%202020-04-16%20at%206.37.57%20AM.png?raw=true" width="80%">

*Fig. 2: Stock Screen*


<img src="https://github.com/johncban/sinatra-stocky/blob/master/ScreenShot/Screen%20Shot%202020-04-16%20at%206.38.20%20AM.png?raw=true" width="80%">

*Fig. 3: Save Stock Symbol*

In Figure 1, the ```post '/stock/new'``` is being use everytime the user search the stock and inside the route or HTTP method is the IEX API that fetch stock information from ```@quote_symbol``` instance variable then it translates the symbol into company name from ```@company_name``` instance variable.

Figure 2, also utilize the same route as Figure 1. However, it provides more information such as stock current price (```@quote_price```), the stock's 10 day volume (```@sv10```) and earnings per share (```@eps```). In addition, it provides the news about the stock using newsapi through ```@data``` instance variable.

In Figure 3, it let the user know to save the stock of his/her choice thorugh ```post '/stocks'``` route that check the form if its entry is valid, the portfolio matches with the existing portfolio name thorugh ```@portfolio = @user.portfolios.find_or_create_by(portfolioname:params[:portfolioname])```; otherwise, if the form requirement does not met the user will stay on the same page with its flag message informing the user about the error.



```
require 'pry'


class StockController < ApplicationController

    alpha_token = ""
    iex_token = ""


    ...


    post '/stocks/new' do
        # IEX - Stock Screening -- begin
        qt = params[:stock_search].to_s
        pn = params[:portfolioname]
        client = IEX::Api::Client.new(
                publishable_token: iex_token,
                endpoint: 'https://sandbox.iexapis.com/v1'
        )
        
        if qt == "" || pn == ""
                flash[:qt_empty] = "Please Enter Stock Symbol or Portfolio."
        else 
                quote = client.quote(qt)

                @quote_symbol = qt
                @q_price = quote.latest_price

                key_stats = client.key_stats(qt)

                @company_name = key_stats.company_name

                @quote_price = "Current Price $#{@q_price}"
                @symbol_price = @q_price
        
        
                @sv10 = "10 Day Volume:  #{key_stats.avg_10_volume}"
                @sv30 = "30 Day Volume:  #{key_stats.avg_30_volume}"
        
                @low = "52 Week Low #{key_stats.week_52_low_dollar}" 
                @high = "52 Week High #{key_stats.week_52_high_dollar}"
        
                @eps = "Earnings Per Share (EPS)  #{key_stats.ttm_eps}"
                @five_change = "Five Day Price Percentage Change:  #{key_stats.day_5_change_percent_s}"

                # IEX - Stock Screening -- end        
        end

       
        url = "https://newsapi.org/v2/everything?q=#{@company_name}&apiKey="
        resp = Net::HTTP.get_response(URI.parse(url))
        @data = JSON.parse(resp.body)


        erb :'stocks/new_stock'
    end

    
    get '/stocks/:id' do
        if logged_in?
                @stock = Stock.find(params[:id])
                
                sym = @stock.stock

                client = IEX::Api::Client.new(
                        publishable_token: iex_token,
                        endpoint: 'https://sandbox.iexapis.com/v1'
                )
                quote = client.quote(sym)
                @q_price = quote.latest_price

                key_stats = client.key_stats(sym)
                @company_name = key_stats.company_name

                client = Alphavantage::Client.new key: alpha_token
                stocks_found = client.search keywords: sym
                @stock_type = stocks_found.stocks[0].type

                total = @q_price * @stock.quantity

                @o_total = "Total Cost   $#{total.round()}"

        
                timeseries = Alphavantage::Timeseries.new symbol: sym, key: alpha_token
                @stock_vol = timeseries.volume
                @stock_high = timeseries.high
                @stock_low = timeseries.low
                        
                erb :'stocks/user_stock_info'
        else
                not_log_redirect
        end
    end

    get '/stocks/:id/edit' do
        if logged_in?
                @stock = Stock.find(params[:id])
                @portfolio = Portfolio.find(@stock.portfolio_id)
                sym = @stock.stock

                client = IEX::Api::Client.new(
                        publishable_token: iex_token,
                        endpoint: 'https://sandbox.iexapis.com/v1'
                )

                quote = client.quote(sym)
                @q_price = quote.latest_price

                key_stats = client.key_stats(sym)
                @company_name = key_stats.company_name

                cl = Alphavantage::Client.new key: alpha_token
                stocks_found = cl.search keywords: sym
                @stock_type = stocks_found.stocks[0].type

                total = @q_price * @stock.quantity

                @o_total = "#{total.round()}"


                if @stock.user_id == current_user.id
                        erb :'stocks/stock_edit'
                else
                        portfolio_redirect
                end
        else
                not_log_redirect
        end
    end

    post '/stocks/:id/edit' do
        @stock = Stock.find(params[:id])
        if !params[:stock_search].empty?
                # IEX - Stock Screening -- begin
                qt = params[:stock_search].to_s

                client = IEX::Api::Client.new(
                        publishable_token: iex_token,
                        endpoint: 'https://sandbox.iexapis.com/v1'
                )

                quote = client.quote(qt)

                @quote_symbol = qt
                @qp_price = quote.latest_price

                key_stats = client.key_stats(qt)

                @comp_name = key_stats.company_name
                # IEX - Stock Screening -- end

                erb :'stocks/stock_edit'
        else 
                flash[:message_stocks_edit_fail] = "Please enter the Stock Symbol! - Error Code #{response.status}"
                redirect to "/stocks/#{params[:id]}/edit"
        end


    end


    ...



end
```

### Summary
In this blog, it explain the MVC of Sinatra-Stocky from Model to View Controller and the concept project for Sinatra that in spite of its lightweight framework of Ruby, it can be a powerhouse web framework by the use of API. 
If you're insterested to test or try this porject please feel free to clone Sinatra-Stocky's [repo](https://github.com/johncban/sinatra-stocky).







