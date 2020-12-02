---
layout: post
title:      "Function over Flash....to start"
date:       2020-12-02 02:36:19 +0000
permalink:  function_over_flash_to_start
---

 I just submitted my javascript project. The assignment was a single page application that utilized a JS frontend and a Rails backend. There was a strict requirement not to have a page refresh though a user would be creating, reading, updating and deleting "something" from the app. 
 
 What I submitted technically met all of the requirements for the assignment though I recieved some tough but great feedback. Before sharing though, let me walk you through my grandeur idea to paint the picture of what I was thinking:

**What I wanted to create:** My aspirations fell nothing short of a single page application similar to a calendly scheduler, having a focus on service related businesses and customers. No big deal right, I am sure Calendly was created in less than a week and operated flawlessly ( : ) sarcarsm). This SPA (single page application) included having user logon functionality, several relationships between classes such as appointments, locations, technicians, and companies. Also, I wanted to overlay a map function to show locations of the appointment and other key info. 

**First Learning** - focus on creating the core neccesities of your app design. You can always add relationships and complexity later. Get the minimum viable product functional before adding in deisgn elements when you are still learning to develop apps. 

**What I did wrong:** Started with too many relationships, classes and a large scope, not rightsized for delivery within the time allowed. This would be the case of overpromising and underdelivering. 

**Example** Backend Serializer. In my project, I had the option to be able to create my own serializer, utlize Active Model serializers, or go for the ultra-fast Fast_JSON serializer.

What is a serializer? A serilizer is used for several purposes, including DRYing up our code. A serializer converts objects into JSON, removes duplication and messy code needed for customizing how the data will look, and seperates concern away from the controller
.
![](https://api-platform.com/docs/core/serialization/)

An example of this can be seen below when you have multiple classes and relationships:

```
render json: appointment.to_json(:include => {
     :technician => {:only => [:name, :title]},
     :location => {:only => [:address, :city]}
   }, :except => [:updated_at])

```
You can imagine as the more complex your project becomes, this can be a challenge to manage. A serializer pulls the above out of the controller into a new serializer folder within your project.

A custom serializer can be created and used in cases where you need data in a specific format when converted to JSON. First step is to create an initializer and passing in an argument of an object.

```
class AppointmentSerializer
 
 def initialize(appointment_object)
   @appointment = appointment_object
 end

```

Next step is to write a method that handles what you want to include or exclude, managing the data being sent to your frontend or when recieving HTTP requests.

```
 def to_serialized_json
   @appointment.to_json(:include => {
     :technician => {:only => [:name, :title]},
     :location => {:only => [:laddress, :city]}
   }, :except => [:updated_at])
 end
```

Last thing to do is clean up your contoller:
```
class AppointmentsController < ApplicationController
 def index
   appointments = Appointment.all
   render json: AppointmentSerializer.new(appointments).to_serialized_json
 end
```
Awesome. You can clean up the serializer when your options get a bit more complex but it is nicely tucked away in its own file.

Cheers!









