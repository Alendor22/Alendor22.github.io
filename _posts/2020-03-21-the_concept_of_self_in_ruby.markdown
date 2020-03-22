---
layout: post
title:      "The Concept of Self in Ruby"
date:       2020-03-22 03:08:29 +0000
permalink:  the_concept_of_self_in_ruby
---


     When you learn ruby, a programing language at some point near the beginning of your journey you will come across the keyword"self".  What is "self", is the first question that comes to mind.  The answer to this is not as simple as one would think, and deserves a deep dive into Computer Science itself.  Self in ruby refers to the current object, allowing developers to refer to refer to a particular class or object, self is what is known as a self referential keyword.  With that being said, one would need to know what a class is and what an object is.  In Object-Oriented Programming a class is a blueprint from which objects are created, the object is also known as an instance of a class.  For example an animal is a class, mammal, birds, fish, dogs, cat, and reptiles are instance of the animal class as a whole.  So if a class can manufacture or instantiate an object of itself when one refers to self, it is dependant on the situation\context the self keyword is used.  For example in one context, self may refer to a the parent class and in another contexts self may refer to a particular instance of a class.  No matter which context the code finds itself, one important rule about self must be remembered: self always refers to one and only one object at any given time.
		 
		 On a higher level when self is not in a class, modlue, method, or any code-block, self at a 'top level" (just a snipet of code in a .rb file) still refers to an object in ruby, if you type in self in irb without any other code, self in "main", or the main object.  
		 
```
2.6.1 :001 > self
 => main 

```
```
2.6.1 :002 > name = "joe"
 => "joe" 
2.6.1 :003 > puts "my name is #{name}"
my name is joe
 => nil 
2.6.1 :004 > puts "my name is #{self}"
my name is main
 => nil 
2.6.1 :005 > puts "self class is: #{self.class}"
self class is: Object
 => nil 

```

As you see when self is not in a class it is the main object, which is automaticlly created.  
Below is the class definition of self, when self is in a class, self is equivalent to the parent class itself.  Self here can be thought of as a substitute for the actual class name.  

```
2.6.1 :009 > class Animal
2.6.1 :010?>   puts self
2.6.1 :011?>   end
Animal
 => nil 

```

When self is defined in a class method (a method that refers to that class, but not an individual instance of that class)
the object that precedes the definition of the method, self refers to the parent class object.  When self refers to the receiver, in this case name, the output is just :name, but when you call on Animal.name the output is Self is: Animal

```
2.6.1 :036 > class Animal
2.6.1 :037?>   def self.name
2.6.1 :038?>     puts "Self is: #{self}"
2.6.1 :039?>     end
2.6.1 :040?>   end
 => :name 
2.6.1 :041 > Animal.name
Self is: Animal
 => nil 

```

When self is used inside of an instance method (a method that is called on an instance of the class) self refers to an instance of the class itself as seen below.

```
2.6.1 :063 > class Animal
2.6.1 :064?>   def name
2.6.1 :065?>     end
2.6.1 :066?>   man = Animal.new
2.6.1 :067?>   def name
2.6.1 :068?>     puts "self is: #{self}"
2.6.1 :069?>     end
2.6.1 :070?>   man.name
2.6.1 :071?>   end
self is: #<Animal:0x00007feb5a1260e0>
 => nil 

```

     So with the keyword self it depends on the object receiving self to provide an acurate definition.
