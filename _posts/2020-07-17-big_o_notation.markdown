---
layout: post
title:      "Big O notation"
date:       2020-07-17 19:13:13 -0400
permalink:  big_o_notation
---


This is not curriculm related but i have been looking forward to learning this topic.

big o notation asymptotic analysis if you wanna sound super smart. 

any coder given enough time can solve any problem, what matters though is how the problem is solved
or how effeciently it is solved, and this notation distinguish good code from bad code or good code from greate code.

this topic comes up a lot when we are talking about data structures and algorithms. 

what is good code or how do we write good code? how do you describe it ?

i think good code is described by its readability and scalability, questions come up like can others understand my code,

code than can scale well is measured by the big O notation. 

If we want to bake a cake we need to use some sort of instructions or recipe and that recipe needs to go in an oven 
to finally get the output(the cake).

so as coders we are the ones that give the recipe where recipe here is the code to the computer the oven to get the end result output or the cake. we can have many recipes to bake a cake or many recipes to get one output but how efficent was that recipe? how long did it take to bake the cake with those ingredients ? a lot of questions can be asked here. 


say my recipe looks something like this and here im going to use javascript because in the curriculm that's the next section we are working on and i want to get some practice with it but feel free to use any language you are comfortable with. 

```
const nemo = ['nemo'];
function findNemo(array) {
for (let i = 0; i <array.length; i++) {
  if (array[i] === 'nemo') {
    console.log('Found Nemo!')
  }
}

}

```
findNemo(nemo);**

so here we are looping over an array and checking the array index of [i] where i is 0 is true than console log an output.

so how long did it take to run this code how can we measure the performance of this code and what happens if our array is made out of more than just one string, well for this example lets measure the performance if we only have one string in the array we can do this to calculate the speed of our function, where we use performance.now() in the loop and console log it out to see the speed 

```
const nemo = ['nemo'];


function findNemo(array) {
  let t0 = performance.now();
  for (let i = 0; i < array.length; i++) {
    if (array[i] === 'nemo') {
      console.log('Found NEMO!');
    }
  }
  let t1 = performance.now();
  console.log("Call to find Nemo took " + (t1 - t0) + " milliseconds.");
}

```
this is the output 
Found NEMO!
Call to find Nemo took 0.19500000053085387 milliseconds.
=> undefined

now assume we have a big array  instead of finding nemo 
assuming its 10 fishes 

```
const fish = ['dory', 'bruce', 'marlin', 'nemo'];
const nemo = ['nemo'];
const everyone = ['dory', 'bruce', 'marlin', 'nemo', 'gill', 'bloat', 'nigel', 'squirt', 'darla', 'hank'];


function findNemo2(fish) {
  let t0 = performance.now();
  for (let i = 0; i < fish.length; i++) {
    if (fish[i] === 'nemo') {
      console.log('Found NEMO!');
    }
  }
  let t1 = performance.now();
  console.log("Call to find Nemo took " + (t1 - t0) + " milliseconds.");
}

findNemo2(everyone)
```

this still outputs 0.1 milliseconds because in this day and age our computers are extremly fast but lets up this a notch or 2 or maybe a thousand

```
const large = new Array(1000).fill('nemo');
```
add this to the code and check out the reults, this will take around 7 milliseconds or so which compared to 0.1 milliseconds is a lot longer so here the outcome is as our input grew our functions became slower and slower 

our runtime increased! 
also keep in mind that this is different for every computer depending on the machine or the cpu in the computer is and what else you have running on the computer so there are a lot of factors. 

so coming back to code that can scale here meaning as the number of inputs increases it doesnt constantly slow down.

big o can compare 2 functions and figure out which one is better when it comes to scale regardless of computer difference.


![]https://miro.medium.com/max/2928/1*5ZLci3SuR0zM_QlZOADv8Q.jpeg here is a big-o complexity chart, its ok if you dont understand this this just simply graphs the increase in input to function effiency or slowing down. 

so basically going back to our example what it means as the array increases how many functions or calculations do we have to do thats it! simple. this is what we call algorithmic effiency. 

different functions have different algorithmic effiency. to simply put it for everyone as the number of elements increase what is the effiency so big o conerns us of how many steps or operations the computer has to perform. 

so here we can say that our example is solved in (n) time or more spesifically linear time for whenever the number of elements increase the number of operations increase signafying the [O(n) time] notation or [linear time]. 


cheers ! 

Aniss Soudki 





