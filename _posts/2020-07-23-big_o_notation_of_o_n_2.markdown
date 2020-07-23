---
layout: post
title:      "big O notation of o(n^2)"
date:       2020-07-23 19:52:54 -0400
permalink:  big_o_notation_of_o_n_2
---


we have seen in the last blog the o(n) notation and that is implemented when we have some sort of looping going around.

so now lets see if we have 2 loops going around what is the big o notation there. for that to calculate big o we mulitply the notation of every loop, so if i have 2 nested loops with every one having a big o notation of o(n), the overall big o will be o(n^2) o of n squared like we said before o(n) is linear o of n(squared) is gonna be quadratic time.


the line on an a graph should shoot up on the y axis in this example. 
so the rule of thumb here a databse has o(1)
a loop big o is o(n)
a nested loop notation is o(n^2) the more nesting the higher the power.


```
// log all pairs

const boxes = ['a','b','c','d','e']

function logAllPairsOfBoxes(array) {
// 2 for loops in here
for (let i = 0; i <array.length; i++) {
  for (let j = 0; j <array.length; j++) {
    console.log(array[i],array[j])
    }
  }
}

logAllPairsOfBoxes(boxes)

// o(n) * o(n) = big O notation of o(n^2)
```
the ouput below

```
a a
a b
a c
a d
a e
b a
b b
b c
b d
b e
c a
c b
c c
c d
c e
d a
d b
d c
d d
d e
e a
e b
e c
e d
e e
```
