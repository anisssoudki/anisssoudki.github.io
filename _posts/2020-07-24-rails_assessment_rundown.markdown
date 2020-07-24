---
layout: post
title:      "Rails assessment rundown"
date:       2020-07-24 22:56:11 +0000
permalink:  rails_assessment_rundown
---


# 1. Explain the MVC framework.
Model
The Model component corresponds to all the data-related logic that the user works with. This can represent either the data that is being transferred between the View and Controller components or any other business logic-related data. For example, a Customer object will retrieve the customer information from the database, manipulate it and update it data back to the database or use it to render data.

View
The View component is used for all the UI logic of the application. For example, the Customer view will include all the UI components such as text boxes, dropdowns, etc. that the final user interacts with.

Controller
Controllers act as an interface between Model and View components to process all the business logic and incoming requests, manipulate data using the Model component and interact with the Views to render the final output. For example, the Customer controller will handle all the interactions and inputs from the Customer View and update the database using the Customer Model. The same controller will be used to view the Customer data.

### MVC architechture 
Step 1 − The client browser sends request to the MVC Application.

Step 2 − Global.ascx receives this request and performs routing based on the URL of the incoming request using the RouteTable, RouteData, UrlRoutingModule and MvcRouteHandler objects.

Step 3 − This routing operation calls the appropriate controller and executes it using the IControllerFactory object and MvcHandler object's Execute method.

Step 4 − The Controller processes the data using Model and invokes the appropriate method using ControllerActionInvoker object

Step 5 − The processed Model is then passed to the View, which in turn renders the final output.


# 2. How do we access data in our views that we defined in our controllers?

accessing data in the views utilizes the instance variable that was passed in inside of the controller action so for example if we passed in @products = Product.all in the controller index action than we will have access to that instance variable inside of the views so whenever we call <%=@products%> we will see Product.all get rendered.

# 3. What is the difference between “redirect_to” and “render”?
They may seem similar to beginners but they are very different. Render tells Rails which view or asset to show a user, without losing access to any variables defined in the controller action. Redirect is different. The redirect_to method tells your browser to send a request to another URL. render  is basically just opening an html file, versus redirect that is making a get request to another URL.

# 4. What methods do we get from “belongs_to”, “has_many”, “has_many, through” and “has_secure_password”?
* has_secure_password method gives us access to authenticate and password digest and it is accessed after we allow bycrypt and bundle it in our gem files. the password digest field uses an algorythm imbeded in the gem to salt that password with a hash preventing any hacker from reversing the operation to find out what the original password is. 

* whenever we get a model and inherit from application record in rails not that we get access to 690 methods for that model, to test this generate a model call it test and drop to the console and put it Test.methods.count, if we make another model test2 it should have the same amount of methods, assuming we didnt have any attributes. if we set up a has_many belongs to relation between these models the test model now will have 702 methods 12 methods extra because of the associations these methods are the following ([:before_add_for_test2s, :before_add_for_test2s?, :before_add_for_test2s=, :after_add_for_test2s, :after_add_for_test2s=, :after_add_for_test2s?, :before_remove_for_test2s, :before_remove_for_test2s?, :after_remove_for_test2s, :after_remove_for_test2s?, :before_remove_for_test2s=, :after_remove_for_test2s=]) they are relate to that model. assuming we well named model for example user has many articles article belongs to user we can call now User.articles (keep in mind article pluaral) and access the methods inside of articles after we loop thru like the title or whatever attributes are in there
* many to many model will grant us access to more methods in my case, i have a product, category and a productcategory model, after we set the macros in the models im able to call product.categories or product.productcategories. 
* Many-to-many
There are two ways to build a many-to-many relationship.

The first way uses a has_many association with the :through option and a join model, so there are two stages of associations.
```
class Assignment < ActiveRecord::Base
  belongs_to :programmer  # foreign key - programmer_id
  belongs_to :project     # foreign key - project_id
end
class Programmer < ActiveRecord::Base
  has_many :assignments
  has_many :projects, through: :assignments
end
class Project < ActiveRecord::Base
  has_many :assignments
  has_many :programmers, through: :assignments
end
```
For the second way, use has_and_belongs_to_many in both models. This requires a join table that has no corresponding model or primary key.

```
class Programmer < ActiveRecord::Base
  has_and_belongs_to_many :projects       # foreign keys in the join table
end
class Project < ActiveRecord::Base
  has_and_belongs_to_many :programmers    # foreign keys in the join table
end
```

# 5. How do we represent relationships between models in our tables?

the relationship is represented with foreign keys so for example in the user has many articles article belongs to user model there would be a foreign key inside of the articles table called user_id that column refrences the user table id column so than if they are matching that means that article with the user_id 1 belongs to the user with the id 1 

# 6. What are params? What are the two places they come from? 
As a Rails beginner, I found it hard to understand what params is all about. Apparently, I am not alone. RailsGuides does a good job of laying out the basics, but going through another example is always helpful.

As you might have guessed, params is an alias for the parameters method. params comes from ActionController::Base, which is accessed by your application via ApplicationController. Specifically, params refers to the parameters being passed to the controller via a GET or POST request.

In a GET request, params get passed to the controller from the URL in the user’s browser. For example, if our app’s controller looked like

```
def show
  @person = Person.find(params[:name])
end`
```


and the user typed in: http://example.com/person/avi or ( http://example.com/person/1 if we passed id in the params)

then the controller would pass in {:name => “avi”} to the show method, which would set the @person instance variable to the person in the database with the name “avi”.


In a POST request, params will get passed to the controller usually from a form. For example, say our app’s controller looked like


```
def create
  @user = Person.new(params[:person])
end
```
and our form looked like

```
<%= form_for(@person) do |f| %>
<%=f.label :name %>
<%= f.text_field :name %> 
<%=f.label :email %>
<%= f.text_field :email %> 
```
:person => { :name => “Avi”, :email => “avi@example.com”} to the create method. Here, @person = Person.new(params) will become @person = Person.new(name: “Avi”, email: “avi@example.com”)
```

If the user submitted the form with the name “avi” and the email address “avi@example.com” the controller would pass the hash

```
Using params gets a bit more complicated when working with nested attributes. Take the example of a playlist app, where a playlist has_many songs and accepts_nested_attributes for songs.

```
class Playlist < ActiveRecord::Base
  attr_accessible :name, :songs_attributes

  has_many :songs
  accepts_nested_attributes_for :songs
end
```

Here, the params for a playlist will be a nested hash and may look like this after adding a few tracks:


```
params = { :playlist => {
:name => 'pop', :song_attributes => [
{ :artist => 'Micheal Jackson', :track => 'Thriller'},
{ :artist => 'stevie wonder', :track => 'superstition'},
{ :artist => 'U2', :track => 'Beautiful Day'}
]
}}
```

Now, if we wanted to access the track name for the U2 song, the params would look like this:

```
params ['playlist'] ['song_attributes'] [2] ['track'] = "Beautiful Day"

```

# 7. What are sessions?
Sessions enable the application to maintain user-specific state, while users interact with the application. For example, sessions allow users to authenticate once and remain signed in for future requests

Many web applications have an authentication system: a user provides a user name and password, the web application checks them and stores the corresponding user id in the session hash. From now on, the session is valid. On every request the application will load the user, identified by the user id in the session, without the need for new authentication. The session ID in the cookie identifies the session.

Hence, the cookie serves as temporary authentication for the web application. Anyone who seizes a cookie from someone else, may use the web application as this user - with possibly severe consequences. Here are some ways to hijack a session, and their countermeasures:
Most people don't clear out the cookies after working at a public terminal. So if the last user didn't log out of a web application, you would be able to use it as this user. Provide the user with a log-out button in the web application, and make it prominent.

Many cross-site scripting (XSS) exploits aim at obtaining the user's cookie.

Rails uses ActionDispatch::Session::CookieStore as the default session storage.
read more about cookies here -> https://guides.rubyonrails.org/security.html

# 8. What is the flow of your application? (i.e. what triggers your get routes vs post routes or patch routes or delete routes)
if we do rake routes we can easily answe this question, the controller triggers what kind of route is what, using the naming conventions as well as defining the routes inside of the config file we can specify what kind of routes are we allowing or preventing resources + the model name will give us access to all RESTFUL routes. than we can modify them to our needs also we can create custom routes for example how we did with sessions 

```
get '/login', to: 'sessions#login'
  post '/login', to: 'sessions#create'
  delete '/logout', to: 'sessions#logout'

```
specifying the request than the url and than sending it to the controller action so /login will go to the sessions controller login method that we have built in and figure out the logic there. 




# 9. How do you authenticate your users when they log in?


User authentication is a security process that covers all of the human-to-computer interactions that require the user to register and log in. Said more simply, authentication asks each user, “who are you?” and verifies their response.

For my model my users are authenticated with an email and a password, as well as a facebook oauth token whenever they chose to autheicate with facebook. 

# 10. How do you validate that a username is unique?
rails provides us with a macro to validate the username or whatever attribute is passed in, which is easy and said but think outside of the box for a second, what if I have multiple users with the same username trying to sign up at the same exact moment, what happens than who wins the race? and by race here i mean this
A race condition or race hazard is the condition of an electronics, software, or other system where the system's substantive behavior is dependent on the sequence or timing of other uncontrollable events. It becomes a bug when one or more of the possible behaviors is undesirable.

so if we have this little example would probably clarify what i'm aiming to say here,
```
class User < ActiveRecord::Base
  validates :username, uniqueness: true
end
```

Note that this does not guarantee uniqueness, since application-layer checks are inherently prone to race conditions. Rails will perform the uniqueness check before creating the user, so you might have the following order of events.

```
 User 1 Checks to see if their username is available
SELECT * FROM users WHERE username = 'bob';

 User 2 Checks to see if their username is available
SELECT * FROM users WHERE username = 'bob';

 Since no duplicates were found User 1 is created
INSERT INTO users (...,username,...) VALUES (...,'bob',...);

 Since no duplicates were found User 2 is created
INSERT INTO users (...,username,...) VALUES (...,'bob',...);
```

This could happen even if you use transactions. The preferred solution to this problem is to add a unique index at the database level.
```
class AddUniqueIndexOnUsername < ActiveRecord::Migration
  def change
    add_index :users, :username, unique: true
  end
end
```

# 11. Why do I need to check that a resource belongs to the current user in the patch and delete routes?

I mean this make sense because I woudlnt want another user say user2 to modify user1 information or submissions, or delete them, that would be a terrible user experience so the helper methods we build like logged_in? and current_user 
they help us in order to prevent this issue. also at the controller level by saying for example session[user.id] = @article.user_id we are giving permission to that user that owns the article to modify it or delete it. 


# 12. What are URL helpers? Where do they come from?
Hard-coded path: "/posts/#{@post.id}"
Route helper: post_path(@post)

We want to use route helper methods as opposed to hard coding because:

Route helpers are more dynamic since they are methods and not simply strings. This means that if something changes with the route there are many cases where the code itself won't need to be changed at all

Route helper methods help clean up the view and controller code and assist with readability. On a side note, you cannot use these helper methods in your model files

It's more natural to be able to pass arguments into a method as opposed to using string interpolation. For example, post_path(post, opt_in: true) is more readable than "posts/<%= post.id %>?opt_in=true"

Route helpers translate directly into HTML-friendly paths. In other words, if you have any weird characters in your URLs, the route helpers will convert them so they can be read properly by browsers. This includes spaces and characters such as &, %, etc.

link_to(*args, &block) 
Creates a link tag of the given name using a URL created by the set of options. See the valid options in the documentation for url_for. It’s also possible to pass a String instead of an options hash, which generates a link tag that uses the value of the String as the href for the link. Using a :back Symbol instead of an options hash will generate a link to the referrer (a JavaScript back link will be used in place of a referrer if none exists). If nil is passed as the name the value of the link itself will become the name.

read more here -> https://api.rubyonrails.org/v3.2.14/classes/ActionView/Helpers/UrlHelper.html#method-i-_routes_context


# 13. When do we typically use form_for vs. form_tag?
 form_tag generates an HTML form for us and lets you specify options you want for your form.
 In the example below, we use the rails method, url_for, to create a URL we want the form to submit to. This request for making a new post would have a URL of /posts and let Rails know the specific controller this URL is going to be for. Additionally, form_tag in this example uses the method option. Since HTML forms can only use GET and POST verbs, using the method option allows you to submit a form with, lets say, the delete HTTP verb as opposed to only using GET and POST.

```
<%= form_tag url_for(action: ‘create’), method: “post” do %>
<%= label_tag ‘Title’ %>
<%= text_field_tag ‘title’, @post.title %>
<%= label_tag ‘Body’ %>
<%= text_area_tag ‘body’, @post.body %>
<%= label_tag ‘Author’ %>
<%= text_field_tag ‘author’, @post.author %>
<%= submit_tag “Create Post” %>
<% end %>
```




form_for method follows RESTful conventions on its own. It accepts the instance of the model as an argument where it makes assumptions for you (which is why it can be seen to be preferred over form_tag). form_for prefers the argument that you’re passing in to be an active record object. This will easily make a create or edit form.

Example of form_for :

```
<%= form_for @post do |f| %>
<%= f.label :category_name %>
<%= f.text_field :category_name %>
<%= f.text_field :content %>
<%= f.submit %>
<% end %>
```

What’s with the form_with !?
The form_with view helper came out in 2017 with the idea to unify the interface of what form_for AND form_tag can do (so it is expected that form_with will be the one more commonly applied). When you pass it a model, it will act the same way as form_for would.
For example, if you have a Message model :

```
<%= form_with model: Post.new do |f| %>
<%= f.text_field :content %>
<% end %>
```
And if you don’t pass a model, form_with behaves like form_tag :

```
<%= form_with url: posts_path do |f| %>
<%= f.text_field :content %>
<% end %>
```
but both would generate 

```
<form action=”/posts” method=”post” data-remote=”true”>
<input type=”text” name=”content”>
</form>
```

form_with submits are remote by default and attaches the data-remote=“true” to the form. You can change this by using local: true. This is the opposite from the other methods where the default is local and you have to specify remote: true to make it remote.

Another difference between form_with compared to form_for and form_tag is that form_for and form_tag generate automatic ids for the input fields. On the other hand, form_with does not. Ids and classes have to be specified. This isn’t necessarily considered to be a bad thing. It’s commonly thought that this function allows for developers to have freedom in how they get to design their forms.
Lastly, another difference in form_with is that any id and class are not wrapped in the html key.
Example of form_with wrapping id & class in keys :

```
<%= form_with model: @post, id: “custom-id”, class: “custom-class” do |form| %>
<%= form_for @post, html: { id: “custom-id”, class: “custom-class” } do |form| %>
```


# 14. How does omniauth work?

OAuth authentication is commonly used as a way for users to log into third party websites using but not limited to their LinkedIn, Facebook, or Github accounts.
In a nutshell, OAuth allows an authentication provider (e.g. LinkedIn) to give access to a resource owner (e.g. the person with the LinkedIn account) to information that is stored on a resource server (e.g. third party app/website) The diagram below illustrates how this relationship works.
Image for post
The resource owner accesses this information through a client application, but the key with this is that the resource server only allows access if the authentication provider will vouch for the resource owner’s identity.
Let’s say my app needs to add a login with Facebook feature. In this case, Facebook is the oAuth Provider and my app is the client application.
1. The resource server needs to be registered as an approved app with the authentication provider. In the very beginning, the authentication provider will have no idea about the app I’m building. I would have to go to Facebook’s developer site to register my app.
2. On the developer site, I would have to provide information like my app’s name and redirect URL. The redirect URL is important because it’s the page the user is redirected to when the login is successful. Once my app is registered, I receive a client ID and client secret that is unique to my app. When the client ID and secret is configured to my app, a user can sign up and login via Facebook.
3. In the login process, my app sends a request to Facebook with the client ID(used to recognize the client) and Facebook sends back the redirect URL after a successful action.
The best part about this process is that the user who is logging in via their Facebook account will never have to provide their credentials directly to my app.

notes: make sure that we create the additional migrations and add them to the users table than we can define a method facebook or oath in the controller and after we check the params comming from provider we can store them in the database basically, also for rails make sure we create the omniauth folder and import the gems and bundle update.


# 15. When a user makes a request (via form, link, manual change in the url), how does rails know how to handle that request?

The Controller: Receiving the request and generating a response
Okay, so you’ve made your request and now you need to set your controller up to receive it. so if a user make a request to get localhost:3000/products what happens is rails will check if that route is valid if it is it will go to the controller responsible for that route so in this case the products_controller inside of the index action say we have it set up to say @products = Product.all than rails will try to render the template responsible for that controller so it will go and look inside of the views folder if we have a products folder, if we do look inside of that at the index template which is what we call the method in the controller def index end  at that point all we have to do is pass the instance variablew that we defined in the controller to the views/product/index.html.erb (@products) than Products.all will be renders as an active record object where we can call methods on it for example we can loop through it since its an array and display every index in that array and call another methode on it. 

# 16. How do you validate data? When are these validations run?

There are several other ways to validate data before it is saved into your database, including native database constraints, client-side validations and controller-level validations. Here's a summary of the pros and cons:

Database constraints and/or stored procedures make the validation mechanisms database-dependent and can make testing and maintenance more difficult. However, if your database is used by other applications, it may be a good idea to use some constraints at the database level. Additionally, database-level validations can safely handle some things (such as uniqueness in heavily-used tables) that can be difficult to implement otherwise.
Client-side validations can be useful, but are generally unreliable if used alone. If they are implemented using JavaScript, they may be bypassed if JavaScript is turned off in the user's browser. However, if combined with other techniques, client-side validation can be a convenient way to provide users with immediate feedback as they use your site.
Controller-level validations can be tempting to use, but often become unwieldy and difficult to test and maintain. Whenever possible, it's a good idea to keep your controllers skinny, as it will make your application a pleasure to work within the long run.

Before saving an Active Record object, Rails runs your validations. If these validations produce any errors, Rails does not save the object.

You can also run these validations on your own. valid? triggers your validations and returns true if no errors were found in the object, and false otherwise. As you saw above:

After Active Record has performed validations, any errors found can be accessed through the errors instance method, which returns a collection of errors. By definition, an object is valid if this collection is empty after running validations.

Note that an object instantiated with new will not report errors even if it's technically invalid, because validations are automatically run only when the object is saved, such as with the create or save methods.

to reiterate validations at the database level are basically validations through active record in the model.


# 17. Why do we use a join table? What relationship are we setting up?
a joint table is used to set up a many to many relationship a great example are the rails docs to read more on this. the joint table usually contains the foreign keys that reference 2 other models here is an example below
```

class Physician < ApplicationRecord
  has_many :appointments
  has_many :patients, through: :appointments
end
 
class Appointment < ApplicationRecord
  belongs_to :physician
  belongs_to :patient
end
 
class Patient < ApplicationRecord
  has_many :appointments
  has_many :physicians, through: :appointments
end
```


in this case appointment would be the joint table and in the table itself it will have to foreign keys one for patient and another for physician 













# 18. How do we set up nested forms - what do these forms need to include so that we can associate the new object with the existing? (i.e. if we have the nested route: posts/1/comments/new, how do we associate the new comment with post with id of 1?)



to be continued 




resources 
https://www.tutorialspoint.com/mvc_framework/mvc_framework_introduction.htm
https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html
https://flatironschool.com/blog/how-params-works-in-rails
https://guides.rubyonrails.org/security.html
https://www.sitepoint.com/an-in-depth-look-at-basic-rails-routing/
https://learn.co/tracks/online-software-engineering-structured/rails/intro-to-actionview/rails-url-helpers
https://api.rubyonrails.org/v3.2.14/classes/ActionView/Helpers/UrlHelper.html#method-i-_routes_context
https://medium.com/@michellekwong2/form-tag-vs-form-for-vs-form-with-fa6e0ac73aac
https://edgeguides.rubyonrails.org/active_record_validations.html
