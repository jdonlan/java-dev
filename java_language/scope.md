#Scope and Access Levels
The life span of a variable depends on where and how it was declared. Likewise, the accessibility of variables and methods depends on the access level with which they're declared. In this section , we'll talk about the life span, or scope, of variables along with the different access types with which they're defined.

When working with variables, there are four types that we can create. There are static variables (also known as class-level variables), member variables (also known as instance variables), parameters, and local variables. Each one of these variable types has a different life span, more commonly known as scope, within your application. It's important that when building your application, you choose the right type of variable for the job so that you don't lose data too soon, or you don't have extra data hanging around that isn't needed. Below is an explanation of each variable type in descending scope order.

###Static Variables
Static variables have the largest scope of any variable type. Static variables belong to an entire class, regardless of whether or not an instance of that class exists. Static variables are created when your app first launches and they are accessible to all regular and static methods within a class. For more about declaring and using static variables, please read the lesson about using the static keyword.

###Member Variables
Member variables are variables that belong to an instance of a class, also known as an object. Member variables are declared within a class declaration but outside of any methods or static blocks. Since member variables belong to an instance of a class, they are created when the object is created and they're destroyed when the containing object is destroyed. Member variables can be accessed by any non-static method within the same class. It's also important to note that member variables are unique across multiple instances of the same class. This means that if you have an employee class that as a member variable that holds the employee's name, each instance of that employee class can have a different value for their name variable. Only static variables are shared across class instances.

###Parameters
Parameter level variables are the variables declared within the parentheses at the end of a method signature. For instance, if you wanted to create a method that took in a string, you would declare that string inside the parentheses in the parameters section of the method signature. Parameter variables are created when a method is called and they exist for the life of the method call. Once your code reaches the last closing curly brace in the method, the parameter variable falls out of scope and no longer exists.

###Local Variables
Local variables are any variable that is declared inside of a method or other block of code. Local variables only exist within the block of code they were created and within any child blocks of that block. For instance, if a variable is declared inside of a method, it exists for the life of the method. However, if you have an if statement inside that method and you declare a variable inside of that if statement, then that variable will fall out of scope at the end of the if statement instead of the end of the method. An example of this and other variable types can be seen below.

```
public class ScopeExample {
	static final String STATIC_VARIABLE = "static variable";
	private int mMemberVariable;
	
	public ScopeExample(int parameter) { // Start of method
		mMemberVariable = parameter;
	} // parameter falls out of scope
	
	protected void doWork() { // Starting block of code
		int localVariable = 0;
		if(localVariable < mMemberVariable) { // Starting block of code
			int anotherLocalVariable = 10;
			localVariable = anotherLocalVariable * 10;
		} // anotherLocalVariable falls out of scope
	}// localVariable falls out of scope
}
```

In addition to having unique scope, the above variable types also have different naming conventions which make it easy to identify their scope. Static variables are typically declared in all uppercase letters with an underscore separating words in the name. Member variables should start with a lowercase "m" followed by a capital letter. The "m" is used to signify that this is a member variable. Local and parameter variables should both start with a lowercase letter.  Some development firms identify parameter variables with a character such an underscore.  For example:

```
public ScopeExample(int _parameter) { // Start of method
    mMemberVariable = _parameter;
} // parameter falls out of scope
```

