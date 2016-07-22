---
layout: post
title:  "Sinatra Dinner Party"
date:   2016-07-22 15:01:32 -0400
---

*Basic stuff...*

This app is built on the Learn.co foundational lessons for Sinatra and ActiveRecord.

**Sinatra** is a DSL (domain specific language) that helps Rubyists build quick webapps. We're using it to map web actions (get, put, post, etc.) to routes in our database and to combine the power of Ruby with HTML in our views.

**ActiveRecord** is a library for working with relational databases (like sqlite3 in our case) providing ORM. Objects (models in the MVC in this app) map to database tables. ActiveRecord provides us with nifty ways to CRUD in the database without reinventing that particular wheel. Also, super cool... You don't specify the attributes of your models, they get inferred from the database table definitions. It seems like a really nice example of DRY in practice.

Combined, Sinatra and ActiveRecord give us most of the tools we need to link routes to database actions and to serve the data to a view on the web. That's exciting!

*So what about this app?*

I've always wanted to create a little app for "supper club" style reservations where you could go and see what you ate on a particular night, get the recipe, leave comments about dishes, get contact info for other guests, etc, etc... The list goes on an on. 

For this project I thought I'd build a base I could expand on later. I decided to build functionality as follows:

**Users**
Users can sign up, log in, and log out.

**Reservations/Dinners**
Logged in users can view upcoming dinners, create reservations, and view upcoming and past reservations.

**Dishes**
Dinners show a list of their dishes. Users can list every dish they've eaten.

**Comments**
Logged in users can leave comments about dinners and dishes. Users can edit their own comments but not the comments of others.

This resulting in the following ActiveRecord associations:

class User < ActiveRecord::Base  
...  
  has_many :comments
  has_many :reservations
  has_many :dinner_dishes
  has_many :dinners, through: :reservations
  has_many :dishes, through: :dinners
....  
end  

class Reservation < ActiveRecord::Base
  belongs_to :user
  belongs_to :dinner
end

class Dinner < ActiveRecord::Base
  has_many :dinner_dishes
  has_many :dishes, through: :dinner_dishes
  has_many :comments
  has_many :reservations
end

class Dish < ActiveRecord::Base
  has_many :dinner_dishes
  has_many :dinners, through: :dinner_dishes
  has_many :comments
end

class Comment < ActiveRecord::Base
  belongs_to :user
  belongs_to :dinner
  belongs_to :dish
end

At this point, mission is accomplished. What do I need next? An admin user role would be fantastic, with the ability to create dinners and dishes and associate dinners and dishes. Right now this is accomplished via terminal, which is super clunky. Also, a dashboard to manage the number of slots per dinner is a pretty big item. Right now we could potentially be hosting a horde and that doesn't sound fun or feasible.


