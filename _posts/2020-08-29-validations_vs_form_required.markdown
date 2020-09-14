---
layout: post
title:      "Validations vs Form Required"
date:       2020-08-29 15:17:05 -0400
permalink:  validations_vs_form_required
---


Two ways I recently came across to validate an input, using ActiveRecord validations or to utilize HTML forms to require input. My specific use case was around confirming that a name, email and password was valid to be entered into the database and also that the email was unique from other users.

Setting the stage:
In order to utilize validations in your database, you need to install the gem [ActiveRecord](https://guides.rubyonrails.org/active_record_basics.html)

**Validations:**
In a standard MVC app, this validation code would be written on the M (model), which would inherit methods from ActiveRecord::Base. 

```
class User < ActiveRecord::Base
    validates :first_name, :last_name, :email, :password,  presence: true
    validates :email, uniqueness: true
  end
```
This code ensures that the attributes first_name, last_name, email, and password were valid by checking the presence, or any user input, was entered into the form. This ensures that your database is maintained and users cannot bypass the input. Secondly, we add the uniqueness validation to ensure that inputs persisted to the database have a unique input and avoid duplication. 

The validation will occur when the object is saved to the database. The return value will the the saved object if successful or false, signaling that there was an error. There are additional ways to address this concern, but the ease of this code and the effectiveness is tough to beat. An additional benefit to utilizing this method is that an error message can be flashed to the user signaling the item needing to be addressed.

**HTML Form Required**
Another simple way to protect the data that is saved to your database is to incorporate 'required' when creating your data entry forms. This is reflected in the code:

```
<h3>Sign Up Below:</h3> 
<form action="/users/signup" method="post">
<label for="first_name"> First Name </label>
<input id="first_name" type="text" name="first_name" required/> <br>
<label for="last_name"> Last Name </label>
<input id="last_name" type="text" name="last_name" required/> <br>
<label for="email"> Email </label>
<input id="email" type="text" name="email" required/> <br>
<label for="password"> Password </label>
<input id="password" type="password" name="password" required/> <br> <br>
<input type="submit" value="Submit"/>
</form>
```
An advantage to utilizing this method would be the direct feedback to the user before the page is refreshed. The disadvantage is that the user can bypass this requirement with some sophisticated measures in editing your html code.

When inspecting the html code, this:
```
<input id="name" type="text" name="name" required="">
```
could become this:
```
<input id="name" type="text" name="name">
```
with a delete keystroke. This bypasses the requirement for the form and could introduce unwanted entries into your database.

This example of validations was directly pulled out from the programming of my first MVC app using the SInatra framework. One of the biggest challenges that I have found in learning to code, and this particular project, was actually **understanding the additional functionality of the framework or gems**. I feel it is absolutely imperative to review and orient yourself to the library documentation of the developer. A great and thorough example of Sinatra documentation can be found [here](http://sinatrarb.com/intro.html). 

In exploring this site, you are quickly able to determine the intention behind the developers reason for creating the library and the functionality it can provide. For example, you quickly understand that Sinatra is intended to make it much easier to handle HTTP requests and to generate responses by directing the requests to specific routes.

```
get '/' do
  .. show something ..
end

post '/' do
  .. create something ..
end

put '/' do
  .. replace something ..
end

patch '/' do
  .. modify something ..
end

delete '/' do
  .. annihilate something ..
end
```

Don't be afraid to dig in and explore all the resources at your fingertips. Hope this example of how to ensure valid data is entered into your app helps in your journey. Good luck coding!
