---
layout: post
title:      "Return values and enumerables"
date:       2020-04-03 01:07:42 +0000
permalink:  return_values_and_enumerables
---


Method returns :

Methods will always return exactly one single thing(an object), the return object can be anything ex:
number, boolean, string, array, even nil ...  in case of nil means nothing, but in ruby its still an object. in a method if there is several statements, assuming they are independent then a method will return the value of the last evaluated statement
an example would be:

 def show_me_a_value
 x = 1 
 y = true
 end
this will retrun true since the last statement is y = true.
in a case where we use the return keyword inside of the method we are telling the method
to return that first statement 

def show_me_a_value
 return x = 1 
 y = true
 end
 to recap using the return keyword is called explicit return, it allows us as developpers to explicitly stop the execution flow of a mehtod and return a spesific value think in terms of making a method that uses if else statements if condition than return something and than the method
 does not continue to evaluate the rest of the code, same as if we use the return keyword on an early statement. 
 on the other hand we have implicit return when return isn’t explicitly called within a method then Ruby returns the value of the last executed instruction in the method.

 The return keyword can only be used within a method if we go in the irb and enter return 2 for example or trying to call return inside of a block but outside of a method
 Array.new.each { |n| return n } those two examples will result in:
  a localJumpError(unexpected return)
  but if we call return inside of a block within a method than that would be considered valid syntax.

enumerables differences and return values:

  (each, collect(map)as well as select and find_all) are all reffered to enumerable methods, they are iterators that we can call on a collection and array or a hash. 
  the each method will basically iterate thru an array but it will not change the array so for example
  assume we want to square an array of numbers 
  array = [1,2,3]
  array.each { |number| puts number ** 2} this will puts => 1,4,9 but the return is [1,2,3]
  so in conclusion the each method although it iterates thru each element and makes the changes we tell it to, it doesnt save the results.
  if we want to use each to get a return value of the squared numbers we can always create a new array and push the results in so we can use this trick

  results_array = []
  array.each do |number|
  puts number ** 2
results_array << num ** 2
end
this works but its more code, instead what we can do is use the collect enumerator method.
array = [1,2,3,4,5]
array.collect do|num|
puts num ** 2
num ** 2
end
this right here will return a new array with the modified elements.
map is basically same as collect.
select and find_all is best used when you need to iterate over an array or hash but only need the elements that return true when tested. select and find_all are basically identical when it comes to working with arrays.
an example of using those is as follows
   array = [1,2,3,4,5,6]
array.select {|num| num.even?} => [2,4,6]
array.find_all {|num| num.even?} => [2,4,6]
the difference between those will come handy when we are working with hashes find_all will return
the key:value pairing as a nested array but select is different because it will return the value in the form of the original input which in this case is a hash

example is as follows: 
     hash= {a:1,b:2,c:3,d:4,e:5,f:6}
hash.select {|key,value| value.even?} => {:b=>2, :d=>4, :f=>6}
hash.find_all {|key,value| value.even?} =>[[:b, 2],[:d, 4],[:f, 6]]

article readings
https://medium.com/@decode2018/when-to-use-each-collect-map-select-and-find-all-methods-in-ruby-a6be7c6e0b13

https://medium.com/rubycademy/the-return-keyword-in-ruby-df0a7f578fcb

http://ruby-for-beginners.rubymonstas.org/writing_methods/return_values.html
