---
layout: post
title:      "Rails E-Haggle"
date:       2020-07-13 19:56:42 -0400
permalink:  rails_e-haggle
---


I build a website called E-haggle, using rails, this website took a lot of time to build and a lot of thinking and debugging a lot of errors. 
I have learned so much from building this website such as mvc in rails using rails methods that are available because of active record ando so much more, rails was built on top of a lot of different gems or librairies and so we got this amazing backend framework that simplifies mvc process for us. 

my models are simple in this project i have a user that can log in and out, signup or log in through facebook. 
this user can fill a form to show case a product(which is another model) and inside this product they can chose a category for this product(hence the many to many relationship). the attributes for category are nested inside of the product parameters and so this way i'm able to set a category for my submitted product form. 

I have some hidden routes like my categories route and categories/new that no one has access to besides my admin users. i wrote the back end that stops regular users from going thru these routes also in the views i made sure that they dont have some sort of button or a way to click on there using some helper methods that i made inside of application record those methods can also be used as helper methods by declaring a statement inside of application controller 
called helper methods and naming them here is a little code snippet 

```
class ApplicationController < ActionController::Base
 

    helper_method :current_user, :logged_in?
   
    

    def current_user
      @current_user ||= User.find(session[:user_id]) if session[:user_id]
    end
  
    def logged_in?
      !!current_user 
    end
  
    def require_user
      if !logged_in?
        flash[:err] = "You must be logged in to perform that action"
        redirect_to login_path
      end
    end
  

end
```
as you see here these helper methods work for both controller and views they basically act as helper methods so we could have moved them there but i prefer using the helper_method macro provided in rails instead. but both ways are fine. In any case , I also have a model comment where a user can comment on a product, where it makes sense to put this in say a user likes a certain product and wants to communicate that to another user. also. I used a gem called act_as_votable where this gem gave me a table in my database and several methods that simplified how i can make a product likable or votable by users, I also had to declare the relationship inside of the user module as well as the product module. the set back i had is that whenever we hit the like button it doesn't automatically update so what i did is i redirected to that page where the like will show, not the best practice probably, but it works, i'm assuming that there is some sort of javascript to use where i can render the refresh but im not sure how to use that yet because we have not dove in javascript in the curriculum yet. 

that being said the project also includes another gem, a pagination gem that i am using on the product index page because i'm assuming if i have multiple users and every user is submitting several product my index page is going to be huge so i used the pagination gem to make the views better for the end user. 

and so this project although very simple took a lot of time as well as connecting javascript to rails 6 because, unfortunately the rails 6 does not support the jQuery gem so instead we have to create an scss file inside of the javascript folder in rails 6 and call that or import the front end we wanna use assuming its bootstrap or materialize in my case i used materialize. we also have to change the code line inside of the layout file from what it comes with when we start a new rails application to this -> <%= stylesheet_pack_tag 'application', media: 'all', 'data-turbolinks-track': 'reload' %>

inside of javascript/packs/application.js enter this code 
```
// This file is automatically compiled by Webpack, along with any other files
// present in this directory. You're encouraged to place your actual application logic in
// a relevant structure within app/javascript and only use these pack files to reference
// that code so it'll be compiled.

require("@rails/ujs").start()

require("turbolinks").start()

require("@rails/activestorage").start()

require("channels")



require("jquery")

require ("materialize")

// Uncomment to copy all static images under ../images to the output folder and reference
// them with the image_pack_tag helper in views (e.g <%= image_pack_tag 'rails.png' %>)
// or the `imagePath` JavaScript helper below.
//
// const images = require.context('../images', true)
// const imagePath = (name) => images(name, true)

global.$ = jQuery;

import '../stylesheets/application'
import 'materialize-css/dist/js/materialize'




$(document).on('turbolinks:load', function() {

    $(".dropdown-trigger").dropdown({ hover: true });
  
  });
  $(document).ready(function(){
    $('select').formSelect();
  });

  $('.like-btn').addClass('like-btn');
``
this is spesific to materialize so if your using bootstrap follow that documentation, also if you are using materialize read thru the documentation as there are some things you have to do inside of the layout files to import icons. in any way 
after this what we want to do is go to `confi/webpack/environment.js enter in this code right below 

```
const { environment } = require('@rails/webpacker')

const webpack = require('webpack')
environment.plugins.prepend('Provide',
  new webpack.ProvidePlugin({
    $: 'jquery/src/jquery',
    jQuery: 'jquery/src/jquery'
   
  })
)

module.exports = environment

`this code i got from the people who made the changes to how jquery works and told all the developers how to get it working in rails 6 i didn't just make this up, just did the research. 
from what i understood jQuer is ran thru webpack now and that's why we need this code also make sure you check package json and make sure you have the correct jQuery version in there as if you dont its gonna break everything, it wont even let you start the server and will give an error here is mine again im using materialize if you are using something else it's gonna look different.

```
{
  "name": "e_haggel",
  "private": true,
  "dependencies": {
    "@rails/actioncable": "^6.0.0",
    "@rails/activestorage": "^6.0.0",
    "@rails/ujs": "^6.0.0",
    "@rails/webpacker": "4.2.2",
    "jquery": "^3.5.1",
    "materialize": "^1.0.0",
    "materialize-css": "^1.0.0",
    "popper.js": "^1.16.1",
    "turbolinks": "^5.2.0"
  },
  "version": "0.1.0",
  "devDependencies": {
    "webpack-dev-server": "^3.11.0"
  }
}

```

this should be it the last piece of code should be to import the framework in for materialize this is done inside of app/javascript/(create a folder called stylesheets)/(create a file called application.css) inside enter this code for materialize for bootstrap its gonna look different so you might wanna look that up  
```
@import url(materialize-css/dist/css/materialize);
```
and that's it, if you try this and still have problems if you explore my application i have notes on what i did so check that out as well. 

finally what i want to talk about is what i learned outside of the curriculum which is big o notations and what do they mean for what we are using so basically the database is o(1) a loop big o notation is o(n) 

what this means is querying the database is very fast and efficient o(1), this is why they are so valuable. because if we have a million users and our speed where we find things is not efficient it's gonna take a long time in order for us to get that information. With this being said the database queries whatever they are gonna be o(1) because they work on an indexing system and by indexing it doesn't have to just be like the id of a table we can query by location or whatever and still have the same big O notation. 

but whenever we are in the views and we are trying to render a certain view say if we have to loop thu that, than the notation is o(n) which is linear times, of course there are a lot of factors that govern the render or search speed but in general the measurement for it in big o notation is o(n), for a simpler exercise you can check out my big o blog i explain it with an example there. 

that being said, i really enjoyed building this project and learning about all these different things from the curriculum and outside of the curriclum 
