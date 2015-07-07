#Static
Like Final, the static keyword has a few intricacies in the Java language.  This section will cover proper usage of static and considerations that should be factored when using it.

##Variables and Methods
The most common use for the static keyword is to define static variables and methods. When calling a method or accessing an instance variable, you need to first have an instance of the class that contains the method or variable. You would obtain an instance of the class by creating a new object, then you would call the method or access the variable that you wanted access. By adding the static keyword to your variable or method definition, you no longer need an instance of the containing class. Instead, you would access those static variables and methods using the class name. An example of this can be found below.

###Non Static Example

```
package com.company.android.staticexample;

public class FileUtil {
	// Non-static class variable
	public String mFileLocation;
	
	// Constructor
	public FileUtil() {
		// Must initialize variable when class instance is created.
		mFileLocation = "myDirectory";
	}
	
	// Non-static method declaration
	public void readFiles(String _directory) {
		// Do file reading stuff here
	}
}
```

```
package com.company.android.staticexample;
import android.app.Activity;
import android.os.Bundle;

public class MainActivity extends Activity {
	@Override
	protected void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
		// Create new FileUtil object
		FileUtil util = new FileUtil();
		// Access file location variable with class instance.
		String directory = util.mFileLocation;
		// Call method on class instance
		util.readFiles(directory);
	}
}
```
Notice how an instance of the class must be used to access the variables and methods.

###Static Example

```
package com.company.android.staticexample;

public class FileUtil {
	// Static class variable
	public static String FILE_LOCATION;
	// No constructor needed
	// Static method
	public static void readFiles(String _directory) {
		// Do file reading stuff here.
	}
}
```

Notice the variable name has been changed to all capital letters. It is a common Java coding convention to name static variables with all capital letters. This isn't required, but it's good practice.

```
package com.company.android.staticexample;
import android.app.Activity;
import android.os.Bundle;
public class MainActivity extends Activity {
	@Override
	protected void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
		// Don't need to create a new FileUtil object.
		// Access file location variable with class definition.
		String directory = FileUtil.FILE_LOCATION;
		// Call method on class definition.
		FileUtil.readFiles(directory);
	}
}
```

Notice how instead of creating a new FileUtil object, the variable and method are called directly on the class definition using the class name.

##Usage and Considerations

###Static Variables
Static variables are typically used in one of two scenarios. The first is when you want all instances of a class to have the same value for a certain variable. For instance, if you're doing some debugging then you might want all of your objects to have access to the same log tag (more on this in the debugging section). By making your log tag a static String, you no longer have to redefine it for each instance of your class. 

The other scenario is when you're creating "global" constants. There is no concept of global variables in Java since all variables and methods must belong to a class. However, it's common practice to setup a Globals or Constants class that contains nothing but public static final variables. By making these variables all static, they act as a global variable would, but without actually being global, sort of like a repository for constant values.

It's important to keep in mind that static variables exist outside of any instance of an class. Normally, when you destroy all objects of a given type then that class is no longer using any memory. However, since static variables exist outside of those objects, even if you destroy or null out all instances of a class, the static variables stay in memory. This won't cause much of a problem when you have static variables that are primitive types, but when you start to have large objects contained as static variables then you might want to make sure you clean up that memory when it's not being used by nulling out your statics.

###Static Methods
Static methods have a somewhat different use case than static variables. Whereas static variables are used to have one value for all instances of a class, static methods are used to have a method be accessible without having an instance of the class at all. It's very common practice to use static methods for utility classes and factory methods. Utility classes are any class you create that isn't meant to act on its own data, but rather to act on other data. Take a look back at our FileUtil class from before. The FileUtil class doesn't have any data of its own to act on and is only meant to help other classes load and save file data. Since FileUtil doesn't have any data, we never need to create an instance of the class and we can make all of those helper or utility methods static. 

It's important to note that, even if FileUtil did have member variables associated with it, those member variables are not directly accessible by static methods. The reasoning for this is static methods can be called without an instance of the containing class and member variables only exist within the class instance.

The other common use case for static methods is creating factory methods. A factory method is a static method that is used to create a new instance of the containing class. So, why use a factory method instead of directly calling the constructor for an object? Well sometimes you want to perform the same or similar actions every time you create a new instance of a class. Consider the following class:

```
public class Employee {
	String mFirstName;
	String mLastName;
	int mId;
	
	public Employee() {
		mFirstName = mLastName = "";
		mId = 0;
	}
	
	public void setFirstName(String _first) {
		mFirstName = _first;
	}
	
	public void setLastName(String _last) {
		mLastName = _last;
	}
	
	public void setId(int _id) {
		mId = _id;
	}
}
```

This Employee class doesn't use a factory method for instantiation, so to create two employees and set up their basic information, we'd use the following code:

```
Employee one = new Employee();
one.setFirstName("John");
one.setLastName("Smith");
one.setId(1);
Employee two = new Employee();
two.setFirstName("Ann");
two.setLastName("Thompson");
two.setId(2);
```

This code works out for a while, but sometime down the road we decide we don't want separate methods for setting the first and last name. We instead want one method for setting the full name. Now we'd have to go back and edit our Employee object creation code to just use one method. In our example this is really easy to do, but if we had created hundreds of employees then we'd be in trouble. This is where a factory can help. Consider the below Employee class using a factory method:

```
public class Employee {
	String mFirstName;
	String mLastName;
	int mId;
	
	// Factory method
	public static Employee newEmployee(String _first, 
			String _last, int _id) {
		Employee temp = new Employee();
		temp.setFirstName(_first);
		temp.setLastName(_last);
		temp.setId(_id);
		return temp;
	}
	
	public Employee() {
		mFirstName = mLastName = "";
		mId = 0;
	}
	
	public void setFirstName(String _first) {
		mFirstName = _first;
	}
	
	public void setLastName(String _last) {
		mLastName = _last;
	}
	
	public void setId(int _id) {
		mId = _id;
	}
}
```
Using this factory method to create objects, we can now change the underlying function calls and instead of changing all of the object creation code, we only need to change this one factory method. Using a factory also allows us to cut down on the amount of code we write by wrapping up common object creation code in one method. Consider the below code to create two employees using the factory method we created:

```
Employee one = Employee.newEmployee("John", "Smith", 1);
Employee two = Employee.newEmployee("Ann", "Thompson", 2);
```

Instead of writing four lines of code to create and setup each employee, we now only need to write one line of code. This may only cut out six lines of code in this example, but again, if you were creating hundreds of employees then you could be saving yourself from writing thousands of line of code. Also, if we now wanted to update our Employee class to use one function for setting the name as seen below, we only need to change the Employee class and leave the object creation code alone.

```
public class Employee {
	String mFirstName;
	String mLastName;
	int mId;
	
	// Factory method
	public static Employee newEmployee(String _first, 
			String _last, int _id) {
		Employee temp = new Employee();
		temp.setName(_first, _last);
		temp.setId(_id);
		return temp;
	}
	
	public Employee() {
		mFirstName = mLastName = "";
		mId = 0;
	}
	
	public void setName(String _first, String _last) {
		mFirstName = _first;
		mLastName = _last;
	}
	
	public void setId(int _id) {
		mId = _id;
	}
}
```

##Static Blocks
Static blocks are unnamed blocks of code that are defined using the static keyword followed by opening and closing curly braces. Static blocks are the very first thing that gets executed in your app and are commonly used to initialize static variables. Static blocks have more uses in traditional Java applications, like loading system resources, but they're not used as often in Android. An example of a static block can be seen below.

```
// Uninitialized static variable
public static String COMPANY_NAME;
// Static initialization block
static {
	COMPANY_NAME = "Java Development Co.";
}
```

####References
[http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html](http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html)