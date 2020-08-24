---
layout: post
title:      "Restaurant Supplies Sinatra App"
date:       2020-08-24 01:36:45 -0400
permalink:  restaurant_supplies_sinatra_app
---


I built a very simple Sinatra app.I have been working on it for days.  

Initially, I started working on the project with confidence. The first two days went by smoothly, and then I fell in my code trap. I made it too complicated so my instructor told me that will be a great project for Rails. I stepped back and started the app from scratch.

This app is built for Restaurant Managers, where they could easily access the list of products needed for the restaurant they are managing. My down the road goal is to make Restaurant Supplies App even more functional, once I learn Rails and JavaScript.
The very first step was to `install gem corneal` in the directory where I wanted my project to be. Corneal gem is created by Flatiron Student and it helped tremendously. Once it was installed, a directory structure was simply there. Setting the directory structure is very intimidating and would take me hours to set it up.

My app has two models (User and Product) and they follow `has_many` and `belongs_to associations`. 
The user can Sign Up /Log In. Once they are successfully logged in, that’s when ActiveRecord and Sinatra start shining and showing their powers.

The first requirement was to build an MVC Sinatra Application. Technically speaking, we could create our app in one file, but it would be very messy and hard to debug. Model-View-Controller paradigm provides a separation of concerns where each file has its role.

**Models**- This folder contains the `User` and `Product` model and they inherit from `ActiveRecord:: Base`.

```
class User < ActiveRecord::Base
   
  has_secure_password
  validates :email, presence: true
  validates :email, uniqueness: true
    
    has_many: products
end
```

Those four lines do magic. 
A macro `has_secure_password` works in conjunction with `bcrypt`. Bcrypt is a gem that store a salted hashed version of our user’s password in our database column called `password_digest`. These two together keeps users protected from being hacked.
Validations are used to ensure that only valid data is saved into our database.
```
validates :email, presence: true
validates :email, uniqueness: true
```

These 2 lines of code will first check for the presence and uniqueness of an email. If an email field is empty and not unique the app won’t start and ActiveRecord will not create a new object.
A `has_many` association indicates a one-to-many connection with another model- in my project user has many products. This association indicates that each instance of the model has zero or more instances of another model. On the other side, I have a Product model that `belongs_to` User. Products database will have a column of the `user_id`, we call it foreign key.

**Controllers** – This folder has three files. `ApplicationController`, `UsersController` and `ProductsController`.

`ApplicationController` inherits from `Sinatra:: Base`, and it makes it the main controller, which will hold configuration that enables the session and sets the views. Also, this is the best file for helper methods to be in.

`UsersController` and `ProductControlle`r inherit from `Application Controller` and that’s how they get the functionality from Sinatra and have access to all of the views, sessions, and helper methods.

`UserController` will carry routes that will allow the user model to sign up, log in or log out.
`ProductController` performs a full CRUD action. This is where the Product can be created, read, edited, or deleted.

**Views** – This folders have two subdirectories for users and products. Each will have multiple views pages that will render. This is where HTML, CSS, and Ruby play together.

The most of troubles I had building this app were Syntax related.  When I was working on creating a new product form, I accidentally deleted `e` from `erb :view`. I spent hours trying to figure out why my page wasn’t displaying. Also, missing `/` in my routes was my other headache. 

This project was very fun to work on. As a beginner, it was very satisfactory to be able to use a browser when coding. 
if you would like to see my code you can check my github page [(https://github.com/ValentinaPanic/restataurant-supplies-sinatra-app)] or if you are intersted in seeing the demo video of my app you can check it here [https://www.youtube.com/watch?v=eNc-Rbstimg](http://)
