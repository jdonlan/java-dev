##Methods and Functions
In any well-designed code base you'll notice several named blocks of code which seem to be attached to actions or commands. These named blocks of code are known as methods. Methods are named blocks of code that belong to a class that can be called to execute the code contained within. Let's first take a look at how to declare a method.
###Method Signatures

As seen below, there are four components to a method declaration in Java: access level, return type, method name, and parameters. Together, these four components form what's known as a method signature. The access level component is covered in the next chapter, so we'll focus on the other components here.

```
/*
 * Access Level - public
 * Return Type - int
 * Method Name - addNumbers
 * Parameters - int _x and int _y
 */
public int addNumbers(int _x, int _y) {
	return _x + _y;
}```

####Return Type

The return type for a method specifies what type of data this block of code is going to return after it finishes executing. A return is specified inside of a method where you wish to stop execution and return data. This is done using the return keyword followed by the data you wish to return. Once return is called, the method stops executing and any remaining lines of code in the method won't be called. 

It's important to note that not all methods need to return data. If you wish to create a method that does some work, but doesn't return data, you should specify a return type of void. When using the void return type, your method cannot return data. However, you can still stop execution of your method by using the return keyword, followed by a semicolon.

####Method Name
A method name is the name by which the method will be invoked in code. Like variable names, method names cannot start with a number and can only contain numbers, letters, and underscores. Also, each method name must be unique and should not share a name with any variables in the same class file. It's common practice to name methods starting with a lowercase letter and capitalizing subsequent words in the name. It's also a good practice to name your methods to be descriptive of what they do. For instance, the onCreate() method in Android contains code that is executed when an Activity is first created. Naming methods in this way makes your code self-documenting and easy to read through.

####Parameters
Parameters are any data that you want to pass into a method to be used during execution. These parameters are declared as variables in parentheses after the method name. If you need to pass in multiple parameters, they should each be separated by a comma. Alternatively, you can also choose to pass in no parameters which you would do by leaving the parentheses empty.

##Constructors
There is a special type of method used in the Java language known as a constructor. Constructors are used to create a new instance of the containing object using the new keyword. Additionally, constructors should be used to initialize any member variables in their class. Constructors are a little different from regular methods in that they don't have a return type and they must always be named the same as the containing class. By default, every class has what's known as a default constructor. Default constructors are a constructor that takes in no parameters. If you don't need to do anything special in your constructor, then you can just not write one at all and the system will use the built-in default constructor when creating new instances of that class.

```
public class Student {
	String mName;
	// Default constructor
	public Student() {
		mName = "";
	}
	public Student(String _name) {
		// Initializing mName value
		mName = _name;
	}
}
```
Note* in the above example there are two occurances of public Student().  This is called method overloading, as explained below.

###Calling Methods
Once your method has been declared, you're ready to start calling those methods. The first thing you need to do is get a new instance of the class that contains the methods you want to call. You can do this by using the new keyword and the class's constructor as seen below. Once you have your class instance, or object, you can call your methods using the name of the object, followed by a dot, followed by the name of the method and any parameters.