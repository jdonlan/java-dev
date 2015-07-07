#Generics
Generic classes are a bit more flexible than typical classes since they're setup to work with a range of different data types. In this section, we'll discover what a generic class is, how to tell if a class is generic, and how to create your own generic class.

From time to time it makes sense to have a class handle different types of data based on what you plan on using the class for. For instance, the ArrayList class in Android may hold a bunch of Employee objects, but other times you might want it to hold a group of Company objects. Instead of making a new class for EmployeeArrayList and CompanyArrayList, we instead use what's called a generic class.

Generic classes are a type of class that takes in a data type as a parameter when it's created. This type parameter is specified using angle brackets and the type can change each time a new instance of the class is instantiated. For instance, let's create an ArrayList for Employee objects and another for Company objects.

```
ArrayList<Employee> employees = new ArrayList<Employee>();
ArrayList<Company> companies = new ArrayList<Company>();
```

You'll notice that we're using the same ArrayList class to create both lists and we pass in the Employee or Company type using angle brackets. Having one generic class be able to handle multiple types of data cuts down on having a lot of classes that perform similar tasks. 

Generics also help to cut down on bugs by giving everything a strong type which helps the compiler point out errors. By specifying a type for ArrayList, the compiler will throw an error if you try to add an Employee to the Company list or vice versa.

While generics are great for creating classes that accept multiple types, the passed in type must be an object type. Primitive types (boolean, char, short, int, long, float, double, and byte) cannot be passed in as a generic type. If you want to pass in one of these types, use the object type associated with the primitive you want to use (the one with the capital letter).

##Creating a Generic Class
Generic classes are incredibly useful and the Android SDK contains several generic classes that provide for a wide range of functionality. However, you might want to also create your own generic classes to contain your own generic functionality. In this lesson we'll cover the basics of creating a generic class without going too in-depth. The reasoning for this is, you will very rarely have to create your own generic class and, once you know the basics, the more advanced features of generics are fairly intuitive.

The first thing we need to create a generic is a simple class definition as seen below.

```
public class GenericClass {
	Object mObject;
	public GenericClass() {
	}
}
```

Next we'll add angle brackets to the end of the class name and specify the letter "T" inside of the brackets as shown here.

```
public class GenericClass<T> {
	Object mObject;
	public GenericClass() {
	}
}
```

When using a generic, you include the name of the type inside the angle brackets so you're probably wondering why we put a T in there this time. When declaring a class to be generic, you need to have a way to refer to whatever type is being passed in so that you can use it. In this case, we're using the letter "T" (for "type") to refer to the type being passed in. Since our passed in type is now being referred to as T, we can now substitute T in place of our variable types.

```
// Our passed in type is being referred to as T.
public class GenericClass<T> {
	// Replacing our variable type with the generic identifier.
	T mObject;
	public GenericClass() {
	}
	// Using generic type in a method return.
	public T getObject() {
		return mObject;
	}
	// Using generic type in method parameters.
	public void setObject(T _object) {
		mObject = _object;
	}
}
```

Now when we use our generic, we can pass in the specified type and use it like we would any other generic class.

```
// Declare a new GenericClass using type Employee.
GenericClass<Employee> genericEmployee = new GenericClass<Employee>();
// Set our object to be a new Employee to match the passed in type.
genericEmployee.setObject(new Employee());
// Get our object as an Employee to match the passed in type.
Employee temp = genericEmployee.getObject();
// Can't set our object to be a Company since it was setup to hold an Employee
genericEmployee.setObject(new Company()); // Won't compile
```

####References
[http://docs.oracle.com/javase/tutorial/java/generics/types.html](http://docs.oracle.com/javase/tutorial/java/generics/types.html)