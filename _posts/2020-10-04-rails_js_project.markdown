---
layout: post
title:      "Rails JS Project"
date:       2020-10-05 01:34:50 +0000
permalink:  rails_js_project
---

the rundown on this project is that its a game, that a user with a unique name can play the game has stats like difficulty and time, i tried to keep it as simple as possible, and focus on project requirment and the game itself, the game is a maze i Used a maze generation algorithm to generate a random maze everytime.
the rails side contains basic relationship between user and game user has many games and a game belongs to a user.

since authentication was not required for this project i didnt add a password for the user, but i did make a sign in and sign up to be able to save new users to database or basically find them if they already exist. 

in the javascript side i focused on the requirment doing 3 ajax requests get post and delete, the main focus was on making the game i did get some help since i have never coded out a game the one that i did in the curriculm for the tic tac toe doesnt compare to this one, i also used a library called matter js, its a physicis library that gives access to a physics engine. 

generating the maze was the hardest part, what i did is i started small hard coded a 3 by 3 grid so that in relation to code would be a 2 dimentional array 

```

[
[false,false,false],
[false,false,false],
[false,false,false]
]

```
all the values in the array are set to false the reason is that i wanted to check if i am at that array index, and if i am change the value to true, in that sence thats how the algorithm works when we start all the cells are not visited except for the starting point and then we start visiting nearby cells or neighbor cells, at somepoint we are gonna get to a point
where the cell has already been visited or we cant go anywhere, but there are still cells that are not visited than we have to back track to check all the other cells. now inside of the grid there are walls horizantal walls and vertical walls i made those intor arrays as well and basically how the maze generation works is that whenever i visit a cell im going to pop off the wall that connects the two cells horizantol walls and verticall walls are also arrays.

now fo js side the player and maze are classes also i made the timer a class for seperation of concerns and called the new method whenver im starting a new game. the maze class  there is a instance method that will start the gameevery time the player presses start or next level the attributes automatically change to make the game harder. in the user class the user can basically create a user find himself or delete and delting a user will cascade delelte all the mazes that the user already played.

this project was fun to do, i still have a lot of refactoring to do for sure, i have tried my best on it and really enjoyed it.
