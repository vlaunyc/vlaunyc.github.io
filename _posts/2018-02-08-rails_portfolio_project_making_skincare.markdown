---
layout: post
title:      "Rails Portfolio Project: Making Skincare"
date:       2018-02-08 05:27:17 -0500
permalink:  rails_portfolio_project_making_skincare
---


Making Skincare is a web application created for the DIY natural skincare community. It is a rails web application where users can browse formulas created by members, or create an account and share their own formulas. Formulas can be sorted by categories and skin concerns.

Before starting to code, I first sketch out how I would want my models to relate and what information I would need to save in the database for each model. I like to think about the different ways I would like to organize that information and how the user might want to interact with the information. I then sketch out the pages on post-it notes of how I would want to layout the web app. Thinking through the bigger picture of how everything might be mapped together from index pages to show pages helps me visualize how it will all flow together. It also helps me figure out how many controllers I would need and what routes I need to set up.

I believe this planning step is the most important part because models relationships are the backbone of the application and what would be built on top. I spent a few days sketching out how everything would be mapped and looked at several websites and their layouts for reference. 

<br>
**PROTOTYPE**<br>
<image width="600" src="https://i.imgur.com/uAQt1fq.jpg" style="center"></image>

<br><br>

**MODEL RELATIONSHIPS**<br>
<image width="600"  src="https://i.imgur.com/b6Be2Dr.jpg" frameborder="0"></image>

<br><br>

**ACTIVE RECORD ASSOCIATIONS**

```
User
has_many :formulas

Formula
belongs_to :user
belongs_to :category
has_many :ingredients
has_and_belongs_to_many :skinconcerns
has_attached_file :image #using paperclip gem here

Ingredient
belongs_to :formula

Category
has_many :formulas

Skinconcern
has_and_belongs_to_many :formulas

FormulaSkinconcern
belongs_to :formula
belongs_to :skinconcern

```


I nested formulas inside the parent of user. So you can create a new formula on **/users/1/formulas/new**
or on **/formulas/new** and both would save with current users id. 

```
# config/routes.rb

Rails.application.routes.draw do
  devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks' }

  resources :skinconcerns, only: [:show, :index]
  resources :categories, only: [:show, :index]

  resources :users, only: [:show] do
    resources :formulas
  end

  resources :formulas

  root "home#index"
  
end

```


```
# app/controllers/formulas_controller.rb

# GET /users/#/formulas
# GET /formulas
  def index
    if params[:user_id]
      @formulas = User.find(params[:user_id]).formulas.order("created_at ASC")
    else
      @formulas = Formula.all.order("created_at ASC")
    end
  end
```
<br><br>
After my models were planed out and 'soild', it was much easier for me to move on with the rest of the application. I did have some trouble with getting omniauth to work and was stuck on that for a whole day! But resolved that after realizing that I had required username for my app but facebook no longer provides username in their [Graph API v2.0.](https://developers.facebook.com/docs/graph-api/reference/v2.0/user/#fields) So I just used `name` which is a string for 'The person's full name'.

```
 def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0,20]
      user.username = auth.info.name 
    end
  end
```

<br>

**DEMO VIDEO**

<iframe width="600" height="338" src="https://www.youtube.com/embed/t7a3rZ1mtRw?rel=0&amp;showinfo=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

<br>

**VISUAL DESIGN** <br>

I used Bootswatch [Journal](https://bootswatch.com/journal/). Bootswatch is a collection of themed swatches that you can download for free and drop into your Bootstrap site. I decided to not download the CSS file and just used BootstrapCDN which is a free public content delivery network. You can use BootstrapCDN and load CSS, JavaScript and images remotely, from their servers.

<br>

**IMAGE UPLOADING** <br>

I used the[ Paperclip Gem](https://github.com/thoughtbot/paperclip) which was super easy to use. It's an upload management for ActiveRecord.  You would first need to install [ImageMagick](http://www.imagemagick.org/script/index.php) and then let Paperclip know where it is. Since my app is in development stage, I added ```Paperclip.options[:command_path] = "/usr/local/bin/"``` to my config/environments/development.rb file. 

<br>

**DYNAMIC FORM FEILDS** <br>

While searching for a better way to add ingredients, I came across this article [Rails form fields](https://rbudiharso.wordpress.com/2010/07/07/dynamically-add-and-remove-input-field-in-rails-without-javascript/) which was a good solution on dynamically adding and removing input field in rails without javascript. I was able to implement this solution into my program without any issues and like the way it looks in the app.

<br>
Overall I'm very pleased with the results. I've learned a lot while working on this project! I was constantly getting stuck and researching which eventually guided me to the finished project. I  look forward to the next one!


