---
layout: post
title:      "What the form_for?"
date:       2020-10-05 03:43:18 +0000
permalink:  what_the_form_for
---


Wow....I am not sure how the idea of nested forms threw me for such a loop but learning about form_for made me feel like a crumpled up sticky note that was tossed into the trash (I know, poor analogy, tried to convey the feeling of getting crushed!) 

I am happy to report in this post that I feel like I have had some of those proverbial sticky note wrinkles ironed out and I am writing to share about where I struggled, how I thought about it, and some general guidance on what has been called "one of the more challenging concepts of rails". 

In googling the phrase "form_for", the top returned site was from [APIdock.com](https://apidock.com/rails/ActionView/Helpers/FormHelper/form_for) where they focus on Action View helpers. In skimming the site, the first thing that jumped out to me was on how to structure the helper.

**form_for(record, options = {}, &block) public**

As a newbie coder, that does not really tell me much. Looking at an example got me going in the right direction:

```
<%= form_for :person do |f| %>
  First name: <%= f.text_field :first_name %><br />
  Last name : <%= f.text_field :last_name %><br />
  Biography : <%= f.text_area :biography %><br />
  Admin?    : <%= f.check_box :admin %><br />
  <%= f.submit %>
<% end %>
```

Allright - much better! Seeing the <%= made me immediately confident that where I started using this form on the view (of the MVC) side of the app architecture was the correct. Next, seeing the :person printed after the form_for. This is the model object your concerned with getting more details (or has_many association) in your code (ie; "incorporates the knowledge about the model object").  The variable f yields to the block and used to generate fields bound to this model.

Cool, feeling ok. Next the text_field, text_area, and check_box are additional helper methods that utilize ruby code to generate HTML. Got it.

Now, let me jump into coding this myself.

```
<%= form_for @account, html: {class: "main-form"} do |f| %>
    
    <div class="form-group">
        <%= f.text_field :org_name, list: "org_names_autocomplete", placeholder: "Organization", class: "form-control" %>
            <datalist id="org_names_autocomplete">
                <% Account.all.each do |account| %>
                <option value= <%= "#{account.org_name}" %>>
                <% end %>
            </datalist>
    </div>

    <div class="form-group">   
        <%= f.text_field :website, class: "form-control", placeholder: "Website to submit data info requests" %>
    </div>

    <div class="form-group">   
        <%= f.text_field :toll_free_number, class: "form-control", placeholder: "Toll free number for data info requests" %>
    </div>

```

First off - if you are a newbie like myself, please ignore all the "html",  "class:" and "div" related code. This is for style and making my website look the way I want my audience to see it. You can delete it and still maintain 100% of the functionality of a form_for.

My model object here is the @account, which I generated (Account.new) in the controller having the :new route and fed into this view.  Next, I structured the variable f in the same way as the example code and began coding the fields I want to be bound to this model.

In this example, the first block of code represents an autocomplete function for when a user starts to enter in an org_name. They will have the option of selecting the predetermined org_name or continuing to type a unique org_name at their discretion.

The next two blocks of code are simply using helper methods to request a website and a toll free number in the form of a string from the user.  Feeling good, next the NESTED function.

```
    <div class="form-group">   
        <%= f.text_field :toll_free_number, class: "form-control", placeholder: "Toll free number for data info requests" %>
    </div>

    <div class="form-group">
      <p class="account-question"> What type of information is associated with your account?  </p>
        <%= f.fields_for :digitalprints do |d| %>
          <div class="form-group">
            <%= d.select :kind, digitalprint_options, {} , class: "form-control" %>
          </div>
            <%= d.hidden_field :user_id %>
        <% end %>
    </div>

    <div class="text-center"> 
        <%= f.submit class:"btn btn-primary" %>
    </div>

<% end %>
```

(I copied a duplicative part of the code to show you that this snippet is below the previous noted section).

The nested form here is another helper method called fields_for. This I found to be very similar to form_for in structure. 

**fields_for(record_name, record_object = nil, options = {}, &block) public**

You have an object model (:digitalprints) that is to be associated with your prior object (@account). Where the magic comes in here is how the output back to the controller happens. You will need to incorporate a macro "accepts_nested_attributes_for :digitalprints" that you will have to incorporate on your model.

```
class Account < ApplicationRecord
    has_many :digitalprints	
    has_many :users, through: :digitalprints

    validates_presence_of :org_name, :website, :toll_free_number
    validates_uniqueness_of :org_name

    accepts_nested_attributes_for :digitalprints
```

Ok...how I think about it is that you have a relationship (in this case, an account has many digitalprints). You are looking to gather information on an instance of a digitalprint and have it associated with the instance of the account at the same time. The variable "d" was selected for ease and we continued to code the additional helper methods to gather the ":kind" from the user.  Lastly, the hidden field was used here to pass the user_id back to the controller in the needed structure for setting up the account/digitalprint/user relationship.

Now, let's take a look at the output of this back to the controller. I believe this is the biggest ahah moment, your using this form to structure the data passed back to the controller in a way that makes persisting simple.

```
<ActionController::Parameters {"authenticity_token"=>"QCvQlJioZUfclWZJBQ4neyL3In5eW18kM8TZiJt9dDIsJKZ0OMFhyeXsalwK3xYxwNzdy8ElFVwZcNId3vlloA==", "account"=>{"org_name"=>"Smoogle", "website"=>"www.smoogle.com/privacy", "toll_free_number"=>"1-800-123-4567", "digitalprints_attributes"=>{"0"=>{"kind"=>"Email", "user_id"=>"6"}}}, "commit"=>"Create Account", "controller"=>"accounts", "action"=>"create"} permitted: false>
```

Ok - my account includes a hash of org_name, website, toll_free_number, and digitalprints_attributes. Digitalprints_attributes was generated in the macro you added "accepts_nested_attributes_for". Awesome! 

Now you can persist the new account and new digitalprint with the relationship structured in the way that you need! 

Hope this walkthrough of form_for and nested forms helps in any way. Cheers!
