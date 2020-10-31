---
layout: post
title:      "Obstacles When Creating A Rails APP - ReSupplyz"
date:       2020-10-31 04:14:52 +0000
permalink:  obstacles_when_creating_a_rails_app_-_resupplyz
---


For the Ruby on Rails project, I have decided to build on my Sinatra application. I created an app that will simplify the ordering process between restaurants and their vendors. I named it ReSupplyz.

 I believe that everyone knows how magical Ruby on Rails is. Learning Sinatra first definitely helped a lot when it comes to understanding MVC structure, sessions and cookies, validations, and the process of setting up user functionalities, such as logging in and signing up.
 
 It was easy to go through lessons and labs. It looked like I had a good understanding of it.
 
However, in the first week of the project, I made nothing. Working on this application ended up being incredibly challenging. Mostly because I did not know where to begin.  

I imagined having two models being able to sign in as users. On one side, vendors would create their portfolios and manage their products’ information and on the other side, managers would place orders with distributors. Depending on who they claim to be, they would see different options on the app.

All of the labs and lessons thought us how to work with a User model, so setting up a Session controller should not be complicated if you make sure that you assign `session[:user_id]` to `user.id`.

The first thing I stumble upon was how to implement two models (Manager and Vendor) with their unique IDs into the Sessions Controller. Signing up was not complicated since those models handle a new and a create route in their controllers. So, creating a login form was the first obstacle.

  I thought of using `check_box_tag` in sessions/ new.html.erb view. That way users (Managers and Vendors), will not have to type more validating information upon signing in, but just to check the correct box. If `check_box_tag` params matches the user’s id, email, and password, it will log them in, otherwise, they will get an error.
	
 A checkbox is tailored for accessing a specified attribute (identified by method create in SessionsController) on the Manager and the Vendor object assigned to the template.

After getting a lot of errors, this is the code I ended up using in my app.

```
#SessionsController
def create
    
      if params[:manager_id]
          @manager = Manager.find_by(email: params[:email]) 
                 
          session[:manager_id] = @manager.id if !@manager.nil? && @manager.authenticate(params[:password])
          redirect_to manager_path(@manager)
        
      elsif params[:vendor_id]
          @vendor = Vendor.find_by(email: params[:email]) 
       
          session[:vendor_id] = @vendor.id if !@vendor.nil? && @vendor.authenticate(params[:password])
          redirect_to vendor_path(@vendor)
  
      else
        flash[:message] = "Credentials are incorrect. Try again!"
        render 'sessions/new' 
      end
  end
```

```
#views/sessions/new.html.erb

<%= form_tag({controller: 'sessions', action: 'create', method: 'post'}) do %>
    <%= label_tag :email %><br>
    <%= text_field_tag :email %><br>

    <%= label_tag :password %><br>
    <%= password_field_tag :password %><br>
    <%= label_tag :manager%>
    <%= check_box_tag :manager_id,  false %>
    <%= label_tag :vendor%>
    <%= check_box_tag :vendor_id, false %>
    <br>
    <br>
    <%= submit_tag "Sign In"%>
    <% end %>

```


One of the requirements for this project was the following:

• Include a many-to-many relationship implemented with has_many :through associations. The join table must include a user-submittable attribute — that is to say, some attribute other than its foreign keys that can be submitted by the app's user.

This is my schema showing how I handled this requirement.

```
create_table "orders", force: :cascade do |t|
    t.datetime "delivery_date"
    t.boolean "delivered", default: false
    t.integer "manager_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "product_orders", force: :cascade do |t|
    t.integer "product_id"
    t.integer "order_id"
    t.integer "quantity"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

  create_table "products", force: :cascade do |t|
    t.string "name"
    t.string "description"
    t.string "category"
    t.integer "price"
    t.integer "vendor_id"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end
```


`product_orders` is a join table and it has the quantity attribute besides foreign keys.

Here comes my second obstacle. It took me a bit of research to create a form for a new order. Nested forms were the only reasonable solution.
Implementing them when I have a join table was hard to grasp. 
The trouble in creating a form was how to get the collection of products and add them to the order and add the quantity to each product. 

I found a video on nested forms that is done by Jennifer Hansen and she explained it incredibly. 
 
This is the final form:


```
<%= form_for @order do |f| %>

  <%= f.label :delivery_date %><br>
  <%= f.date_field :delivery_date %><br>

      <%= f.fields_for :product_orders do |pord_fields|%>
      <%= pord_fields.label :product_id, "Products"%>
      <%= pord_fields.collection_select(:product_id, Product.all, :id, :name, include_blank: true)%>
           

      <%= pord_fields.label :quantity%>
      <%= pord_fields.number_field :quantity%><br>
      <%end%>

  <%= f.label :delivered %><br>
  <%= f.check_box :delivered %><br>
  <%= f.submit%>

<%end%>
```



Since `product_orders` is the link between products and orders. Nested form for `product_orders` was the way to get products to the order.  Besides setting the form this way, I had to put this line of code in the Order model
  `accepts_nested_attributes_for :product_orders`

Nested attributes allow you to save attributes on associated records through the parent (@order in my app).
Also, strong params in the OrderController had to be up to date for this form to work.
```
def order_params
        params.require(:order).permit(:delivery_date, :delivered, :manager_id, product_ids: [], product_order_ids: [], product_orders_attributes: [:quantity, :id, :product_id])
 end
```


And I was able to place the order.

The rest was not hard, it was just figuring it out what would be the flow of the app and implementing the code. 

 In the end,  I spent some time on CSS and it was super fun seeing the results right away.
