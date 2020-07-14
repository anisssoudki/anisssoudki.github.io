---
layout: post
title:      "Rails E-Haggle"
date:       2020-07-13 19:56:42 -0400
permalink:  rails_e-haggle
---



-----------------------
notes
assets will load the css and js it will get loaded from layout application link 
channels folder will work with application cable for chat app or messages  
controles folder -> house all controllers 
helpers is for helper function or methods to help with our views
js folder handled by webpack packs folder than js manifest file 
models folder store all models inherite from active record 
views folder includes layout folder and the rest of our views the yield code in application.html.erb will yield all our views

bin folder (binary executable folder )
confi(configuration for application->env defines development production exetera)(credentiels file can be used to accept api )
routes.rb is in config and it defines our routes 
db is to house the database and our migrations and schecmas 
gem file for gems
package.json lists the packages and dependencies using yarn
readme.md (instructions for the application)
.gitignore and .files dont show by default they are hidden files 
------------------------------------------------
version control with Git when we make changes to code we make commit so basically saving with a message with info about the code 
commit copies and save how the code looks so there is a commit history so if there is a mistake we can go back to an earlier version of the code git.scm.com lots of documentation 

-----------------------------------------
Deploying to production on heroku - production hosting service  
https://devcenter.heroku.com/articles/getting-started-with-rails6 (instructions on deployment good for refrence)
we will need heroku cli 

heroku login take us to a website heroku cli might ask for username and password
back to terminal it should say logged in as the user 
make sure we are in the application and the next step is to create a production shell in production env
heroku create will create a shell and give us a link which is the website 
we have to set up database gem for production heroku uses postgress not sqlite3 so we have to change that 
sqlite for development postgress for deployment in gem file group production we have to add the pg gem 

-->group :production do 
gem 'pg'
end

comment out or remove the sqlite gem and add it to development and test 
-->run bundle install --without production 
--> make a commit git add -A git commit -m "make app production ready" git push heroku master 
this will deploy to heroku app 

heroku open to open the app we can change the name of the application 
form app directory 
--> heroku rename (name of app ) ignore parenthentese 
git remote -v to check the new name 

we on heroku and have an application on the live web :) 
-----------------------------------------------------------------------

database's and modules 
crud communicates with sql and databases crud-> create read update delete 
active reacord orm object relational mapper is a middle man that communicates with ruby and the database so we dont hve to type sql queries 
 application record is an additional abstraction from active record and we can use it to inherite from in our created modules in rails v 5 and above
 --------------------------
scaffold generator: will generate table model controller and views (rails magic)
scaffold generator: will basically generate everything for us 
example rails generate scaffold Article title:string description:text 
this will create all this 

invoke  active_record
      create    db/migrate/20200704212452_create_articles.rb      create    app/models/article.rb
      invoke    test_unit
      create      test/models/article_test.rb
      create      test/fixtures/articles.yml
      invoke  resource_route
       route    resources :articles
      invoke  scaffold_controller
      create    app/controllers/articles_controller.rb
      invoke    erb
      create      app/views/articles
      create      app/views/articles/index.html.erb
      create      app/views/articles/edit.html.erb
      create      app/views/articles/show.html.erb
      create      app/views/articles/new.html.erb
      create      app/views/articles/_form.html.erb
      invoke    test_unit
      create      test/controllers/articles_controller_test.rb
      create      test/system/articles_test.rb
      invoke    helper
      create      app/helpers/articles_helper.rb
      invoke      test_unit
      invoke    jbuilder
      create      app/views/articles/index.json.jbuilder
      create      app/views/articles/show.json.jbuilder
      create      app/views/articles/_article.json.jbuilder
      invoke  assets
      invoke    scss
      create      app/assets/stylesheets/articles.scss
      invoke  scss
      create    app/assets/stylesheets/scaffolds.scss

now we can run the migration file rails or rake db:migrate this will create our schema we can adjust the migration file before if we need to 

in db schema this is what will get created 
ActiveRecord::Schema.define(version: 2020_07_04_212452) do

  create_table "articles", force: :cascade do |t|
    t.string "title"
    t.text "description"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end

end

it also creates the articles controller which i will write comments explaining what they do so refer there as well 
-> app/controllers/articles_controller
also an article.rb modul inherits from application record so its already set up to talk to the database
in config routes it generate the resources :articles command which provide all the crud routes 
rails routes --expanded will give us a very nice reading view for the routes
to check it rails routes || grep articles will show us all the articles 

The line resources :articles in the config/routes.rb file provides the following routes:

- index of articles (GET request)

- new article (GET)

- create article (POST)

- edit article (GET)

- update article (PUT and PATCH)

- show article (GET)

- delete article (DELETE)

From UI perspective ->

- index lists all the articles in the articles table in the database of the app

- new article deals with the form to enter in new article details

- create handles the submission of the items in the new article form

- edit article deals with the form to enter edited information for an existing article

- update article deals with the submission of the edit article form

- show article displays an individual article based on selection

- delete article deletes an article from the articles table

-----------------------------------------
now i will manually generate article but instead going to name it something different maybe blog 
instead of using scafflod i will either generate it using controller or resource and manually write everything in 
as practice/ this is only intended for practice its going to end up being a bogus website 

model name: singular (blog)
blog model file name: blog.br snake case singular 
blog model class name: Blog capitalized (class name are capitalized follow camel case convention)
table name: blogs plural as in blogs table 

------------------------------------------------------------------------------
rails generate migration create_blogs
->invoke  active_record
      create    db/migrate/20200704223233_create_blogs.rb
      add attributes than -> rails db:migrate to run the migration 
      we can add new attributes by generating a new migration if we forgot or we need to update a table 

rails generate migration add_description_to_blogs
generates invoke  active_record
      create    db/migrate/20200704224030_add_description_ 
      now we can go on that migration file and add what we need 
      run the migration rails db:migrate than we will get this
       add_column(:blogs, :description, :text)
   -> 0.0028s
== 20200704224030 AddDescriptionToBlogs: migrated (0.0036s
check the schema to make sure its added.
------------------------------------------------------------
we can make a model file called blog.rb in it inherite from the application record (this model gives us getters and setters because of the inheritance from application record)
we can drop to the rails console rails c and test the connection to the database
 Blog.all
   (0.5ms)  SELECT sqlite_version(*)
  Blog Load (0.2ms)  SELECT "blogs".* FROM "blogs" LIMIT ?  
[["LIMIT", 11]]

--> authentication of our blog module right now we can create empty blogs so to prevent this we are going to use validations or constraints using the model file to enforce what can go into the database
 another validation we wanna add is uselessness of blogs so we can enforce minimum length requirment for title and description

https://guides.rubyonrails.org/active_record_validations.html

------------------------------------------------
add the routes in the config we can use resources :blogs
if we restrict some routes we use resources :blogs , only: [:show]
----------------------- 
added index and made links in the index blogs views 

added new and create in routes and def in controller and made the form

https://guides.rubyonrails.org/form_helpers.html

this guide is for rendering forms 
form_with uses ajax https://en.wikipedia.org/wiki/Ajax_(programming) read up on ajax

added flash messages for blog creation if valid that it will notify with a flash message if not than it wil display the restrictions for the user
----------------------------------------------
full crud implemented just need links to everything 
----------------------------------------------------
will not do for this app cause its just a doodle 

here are notes on deploying to production


Production deploy - text follow-up
We used the following steps to deploy our application to production (using Heroku). If you have not set up a Heroku account and deployed the application at the end of section 3 (including all the setup steps like installing Heroku CLI and creating a heroku app), you are encouraged to do so now before moving forward. If you would rather not deploy to production, then you can safely skip this step.

Steps we followed:

- verify Gemfile

- issue the following command in the terminal bundle install --without production

- make a commit of code

-- git add -A

-- git commit -m "helpful message depending on what you are committing to your repo"

To deploy to heroku:

git push heroku master

To push to GitHub repo:

git push origin master

Since we now have a db component to our application with the articles table, we will need to run migrations in production so the production articles table is created. To run migrations at heroku, you can use the following command:

heroku run rails db:migrate

You can preview your production app by issuing the following command from the terminal:

heroku open

or,

You can directly pull up a browser window and paste in the name of your application. It'll take the following format:

https://yourappname.herokuapp.com

-----------------------------------
using yarn we can use the commands yarn add bootstrap and yarn add jquery and yarn add popper 
also after doing that our package json should be updated with these add ons 
copy any bootstrap code like a nav bar to the layout to test if the javascript is working
require bootstrap in the manifest file app/assets/stylesheet/application.css it should be in the pipeline which is the 
app/assets folder inside of application.css enter this line
--> *= require bootstrap
inside of congi/webpack there is a file environment.js  in there we need to enter this code 
to enable javascript
--> const webpack = require("webpack")
 environment.plugins.append("Provide", new webpack.ProvidePlugin({

$: 'jquery',

jQuery: 'jquery',

Popper: ['popper.js', 'default']

}))
than in app/assets/packs/application.js add this line import "bootstrap"
if it doesnt work kill the server save everything restart the terminal and the IDE make sure u save everything 











git add -A 
git commit -m 
git status 
git push origin master

notes:
Idea: e-marketplace where u can view goods or services 


model: user - product - categorie - (product-categorie )->joint-table

user: username email password
product: Name - description - image - price 
categorie: name 
product-categorie: product_id categorie_id user_it (like or favorite attribute)

future: comments-also can be an attr in p-c <--- 
future: add location attribute on product using iframe 

note->the user_id in product categorie is to count the likes or favorites
relationships:
user has many_products 
product belongs to user 
product has many categories through p-c 
categorie has many product through p-c 

this is new to me i looked up how to upload images to the application found out that we have to install something called active storage and migrate it in the database

  rails active_storage:install
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
Copied migration 20200707044643_create_active_storage_tables.active_storage.rb from active_storage
[21:46:43]  e-haggle (master)
// ♥  rails db:migrate
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
== 20200707044643 CreateActiveStorageTables: migrating ========================
-- create_table(:active_storage_blobs, {})
   -> 0.0035s
-- create_table(:active_storage_attachments, {})
   -> 0.0035s
== 20200707044643 CreateActiveStorageTables: migrated (0.0109s) ===============
-->>>>>>the console rendered the above

question:
----->if the blob saves in the database as binary data or another way versus saving the actual url on a server and call it back in an url tag<<-----



after around 8 or 9 hours of trying to figure out how to import jquery for the materialize gem correctly i finally figured it out 
https://medium.com/@guilhermepejon/how-to-install-materialize-css-in-rails-6-0-0-beta2-using-webpack-347c03b7104e
https://www.botreetechnologies.com/blog/introducing-jquery-in-rails-6-using-webpacker 
following these 2 guids it worked with some slight changes that i have notated above the files also make sure to create a stylesheet folder inside of the javascript file in the asset pipeline and import the materialize there using this code @import url(materialize-css/dist/css/materialize);
 resolved_paths: ['app/assets'] this code inside webpacker.yml 
 u can chose to import the stylesheet assets in the app/stylesheets we have to import that in the javascript/stylesheets(that we created)/application.css (import bootstrap or whatever ur using for css and also @import "stylesheets/app.css(or whatever u named that file) ) and all of this because of the rails update cause rails is using webpacker now so we have to kind of go around and manually set everything up also note whatever css file we create we have to import it inside of the main css app in the stylesheets so it works
https://www.youtube.com/watch?v=88lFAksFvJI follow this video



moving on working on the user model I saw some stuff about validating the user email in a certain hashing method 
and i also found a good resource like a website to check the format for email 
https://rubular.com/ this website here will check for the expression if it is valid or not
i might attach a picture if anyone is confused but basically just follow the format and u have to escape the / and put an i at the end 
if u do it incorrectly the website will correct you. i found it to be very helpful

working on testing if we run command rails test it should let us know if we have tests or not i put some basic tests for 
categories in the model test folder also turns out we can generate a test scaffold by using the following command
rails generate test_unit:scaffold category but instead i just generated it automatically with the controller 
went ahead and wrote the scaffold tests but those usually get auto generated with the scaffold unit test generator and  now im going to try and work on the controller test integration, i might alter them a bit depending on my project

going to try and add admin functionality to users model
here im going to add an extra column admin and set it base value to false 
so the table will look something like this 

id|username|email|password|admin
---------------------------------
1 |  user1 |     | pass   |false
---------------------------------
2 |        | ema |        |false
---------------------------------
3 |        |     |        |false
where no one starts as an admin to begin with
after creating and migrating the admin to users table we can go check in the console 
do Users.all we should have an additional attribute admin: false that is always false
2.6.1 :001 > User.all
   (0.6ms)  SELECT sqlite_version(*)
  User Load (0.3ms)  SELECT "users".* FROM "users" LIMIT ?  [["LIMIT", 11]]
 => #<ActiveRecord::Relation [#<User id: 2, username: "updating from user1", email: "aniss@gmail.com", password_digest: [FILTERED], created_at: "2020-07-09 22:31:34", updated_at: "2020-07-09 22:37:00", admin: false>, #<User id: 
3, username: "i will delete", email: "iwilldelete@mail.com", password_digest: [FILTERED], created_at: "2020-07-09 
22:45:20", updated_at: "2020-07-09 22:45:20", admin: false>]>
2.6.1 :002 > user = User.first
  User Load (0.3ms)  SELECT "users".* FROM "users" ORDER BY "users"."id" ASC LIMIT ?  [["LIMIT", 1]]
 => #<User id: 2, username: "updating from user1", email: "aniss@gmail.com", password_digest: [FILTERED], created_at: "2020-07-09 22:31:34", updated_at: "2020-07-09 22:37:00", admin: false>
2.6.1 :003 > user.admin?
 => false 
in the console we can do user.toggle!(:admin) so that the local variable we have defined will toggle the admin value on 
so that user now is an admin
this will look as follows


2.6.1 :001 > user = User.find(4)
   (0.6ms)  SELECT sqlite_version(*)
  User Load (0.3ms)  SELECT "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 4], ["LIMIT", 1]]
 => #<User id: 4, username: "ADMIN", email: "admin@mail.com", password_digest: [FILTERED], created_at: "2020-07-09 23:52:09", updated_at: "2020-07-09 23:52:09", admin: false>
2.6.1 :002 > user.toggle!(:admin)
   (0.1ms)  begin transaction
  User Update (5.8ms)  UPDATE "users" SET "updated_at" = ?, "admin" = ? WHERE "users"."id" = ?  [["updated_at", "2020-07-09 23:52:47.280914"], ["admin", 1], ["id", 4]]
   (9.4ms)  commit transaction
 => true
so now we can use this feature and add special previlages for admins using views and controllers

i used a gem called acts_as_votable to implement the like button functionality 
i tried without the gem first by trying to add a form in the product show route 
i figured i needed jquery or some sort of dome manipulation in order to implement the functionality so i tried to pass the params and give a class in the views than manipulate it by using an onclick function in the application js file but that didnt seem to work than i figured it should be its own table to where its toggeled and figured out that that is going to take me a long time to implement so i searched for a gem and came accross one that is compatible with rails 6 because some are outdated or conflict with the active storage versions in any case i followed some instructions on where i got the code what we need to do is create a like method in there implement an conditional for the like and unlike than we need to add the code to our user and product modules in order for the backend methods to take effect than make a file that uses jquery for the dom manipulation and render it in the show page i had a problem where i needed to refresh the page for the like to show so i just redirected_to that page after the click and that worked fine 

in any case i have gotten this far i still have couple of things to work on 
like the facebook omnioath and cleaning up the code a little but so far this project is massive compared to sinatra...

