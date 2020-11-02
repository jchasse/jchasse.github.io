---
layout: post
title:      "Async[hronous] awhat?"
date:       2020-11-02 02:58:07 +0000
permalink:  async_hronous_awhat
---


Javascript is a single threaded programming language. So what do we do when we have a user visits our app that happens to make a large data request (through an API) that blocks the rest of the page from loading? 

Thanks to **Asynchronous javascript** (callbacks, promises, and async/await) functionality, our user will have plenty to focus on while the data is being returned to populate the rest of the app/site. 

Here we go! When developing a recent project, I came across a situation where I was making multiple fetch requests. In of itself, making multiple fetch requests is not an issue. I came across very inconsistent errors and sometimes no errors. The reason being, depending on how fast your server is running or the amount of data needing to be processed, the promises being returned were not linear in how they were sent back from my server.

Here is what I was trying to have my function do:

```
function initialization() {
User.createUser()
Location.createLocation()
Service.createService()
resetForm()
Location.displayLocations()
}
```

Each one of the create functions fired a fetch request to the server to persist the inputs from the form to the database. I had 3 in a row; user, location, and service. Running the code this way was very inconsistent, causing returns ranging from response errors to it actually working a time or two. Debugging this was extremely challenging, since I would track down what I thought was introducing the bug to only have another one pop up.

After consultation and reviwing our curriculum around asynchronous js with fetch requests and then functions, I was able to generate the following code to chain these fetch requests together. This solved my specific issue of having my simple local server overwhelmed by 3 fetch requests at the same time due to asynchronous behavior.

The code that consistently worked in persisted the inputs from the form is as follows:

```
        User.createUser()
        .then( newUser => Location.createLocation(newUser))
        .then( () => Service.createService())
        .then( () => resetForm())
        .then( () => Location.displayLocations())
				
```

Utilizing the then function, I was able to limit the fetch request asynchronous behaviour that sends this request (and other fetches) to the task queue by requiring the next function to wait until the prior return value/error is recieved.

This is an area that I believe can make a programmer great in javascript and look forward to continuing to challenge myself in learning asynchronous functions.
