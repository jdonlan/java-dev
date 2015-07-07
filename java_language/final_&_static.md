#Final
The final keyword has two uses in the Java language, both serving similar yet distinctly different purposes. The first use is to make the value of a variable immutable. The second purpose is to make a method or class immutable. These purposes may seem like the same thing, but they're applied in different ways as you'll see below.

##Final Variables
By making a variable final, you're signaling to the compiler that, once initialized, the value of that variable is never going to change. Therefore, if you were to create a final integer and initialize its value to four, that integer will always be equal to four. If you try and change the value of that variable, the compiler will give you an error and your project won't compile. Another thing the compiler does is make subtle background optimizations when a variable is set to final. Since the compiler knows that the value will never change, it doesn't have to write any operator code into your executable to allow for the variable to change. It's a minor optimization, but helpful if you're writing large applications.

There's one thing that needs to be clarified with final variables. **While the value of a final variable is immutable, the state of a final variable can still be changed.** 

This means that if you create a new final object variable, you cannot reassign that variable to be a different object. However, you can still call functions on that object which may change the object's internal member variables which gives the object a different state. Also, member variables that are marked as final must be declared immediately or, at the latest, in the constructor for the containing class. An example of this can be seen below.

```
// Initialized final variable
final int finalInt = 0;
// Uninitialized final variable
final float finalFloat;
// Initializing uninitialized final variable
finalFloat = 1.0f;
// Initialized final employee named Joe Smith
final Employee employeeJoe = new Employee("Joe", "Smith");
// Changing the state of joe, but not the value.
// The variable value is the same, but the internal
// values have changed.
employeeJoe.setName("Mary", "Thompson");
// Attempting to change the variable value will
// give you a compiler error.
employeeJoe = new Employee("Mary", "Thompson"); // Won't compile.
```

##Final Methods and Classes
By marking a method or class as final, you're signaling to the compiler that you don't want the class or method to be extensible. For classes, this means you can't extend them. For methods, this means that you can't override them. Both of these topics have to do with inheritance, which you'll learn about in the following section, so the below explanations are going to be kept rather basic.

When declaring a new class, if you mark the class as final, you won't be able to use that class with the extends keyword. The reason for doing this is that the functionality of the class may be very specific and changing any class functionality could cause unintended results. By not allowing another class to extend your class, you prevent that class from altering the functionality of the original class.

```public final class FinalExample {```

Final classes are great if you don't want any of the class functionality to change. However, what if you only want a certain part of the class functionality to stay the same? If you make the class final, all class functionality becomes immutable. To make only a portion of a class immutable, you can instead mark one or more methods as being final. 

In Android, you'll likely be using the onCreate() method of the Activity class since it's an important lifecycle callback to implement. If you look at the method, you'll notice an @Override annotation above the method signature. This means that your implementation of onCreate() is masking the implementation defined in the Activity class.

If you don't want a method to be overridden and changed like this, mark it as final. Trying to extend a final class or override a final method will cause a compiler error and your project will not compile until you fix the errors.

```
public final void finalMethodExample() {
	// Functionality that shouldn't change.
}
```

####References
[http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html](http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)