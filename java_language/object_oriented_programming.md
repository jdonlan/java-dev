#Object Oriented Programming
Object Oriented Programming refers to a set of commonly accepted best practices when it comes to modern development.  The core foundation of this practice is think of computer programs in terms of distinct objects with properties and behaviors much like objects in real life.  

An animal, for example, has certain properties such as its genus and species.  Animals have behaviors such as cell replication, reproductive cycles, and energy consumption.  While different living beings will do all of these things differently, they are all basic functions of life.

The three primary tenants of Object Oriented Programming (OOP for short) are Encapsulation, Inheritance, and Polymorphism.

##Encapsulation
Encapsulation is the most fundamental of these concepts.  It represents the collection of these behaviors and properties into single components known as objects.  In Java, these are generally represented by a Class with custom variables and methods which house these pieces of data and functionality.

##Polymorphism
Polymorphism is the concept that certain functionality will react differently to different stimulus.  For example, an animal may jump differently if jumping vertically versus jumping across a hole, versus jumping over an object.  All three behaviors are technically jumping, but might include different parameters when executed in code such as an X delta, X and Y delta, or perhaps an Object reference respectively. In Java/Android, Polymorphism is primarily accomplished with method overloading as covered in a previous section.

##Inheritance
Inheritance is the final piece of the puzzle, and perhaps the most complex. Referencing the animal example above, there are many types of animals.  A snake, cat, and ant all are animals, and thus share those common traits such as reproduction, energy consumption, and cell replication. That said, they are all accomplished in slightly different ways.  In addition, each of these animals has their own custom behaviors and traits that are unique to them and not common across all animals.  As such, the cat could be considered to inherit from animal, but not vice versa. 

In this example, the animal represents what is known as a super class.  The cat would represent what is known as a subclass.  In essence, the subclass "extends" the super class, adding or modifying the behaviors and traits of the extended class in the process.

###Subclasses
Subclasses share a type with the extended class. In Android, our MainActivity extends Activity and thus our MainActivity is an Activity. When you extend a class, you also inherit all of the member variables and methods from the extended class (super). This enables subclasses to call methods and access member variables of the super class without the need of creating or initializing them within the subclass itself. 

It should be noted that the inverse is not true. Anything defined in the subclass is only accessible to that class. For example, if MainActivity extends Activity, it can access the methods from Activity while Activity cannot access the methods of MainActivity.

This concept of inheritance is much easier to understand if you think of it as a parent/child relationship. The extending class (subclass) being the child while the extended class (super class) is the parent. Children inherit certain traits and abilities from their parents, but parents don't inherit traits or abilities from their children.

Child classes can access class members of their parents by using the super keyword. This can be used within a subclass to call methods or access class data from the super class. For instance, if you override onCreate() in an Activity, you still need to call the super class onCreate() method so that the system can do all the necessary setup. To call this method, you could call super.onCreate().

In addition to inheriting all of the non-private methods and member variables from the super class, subclasses can also override methods in the super class, creating custom or extended functionality based on the original.
