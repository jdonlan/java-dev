##Methods and Functions
In any well-designed code base you'll notice several named blocks of code which seem to be attached to actions or commands. These named blocks of code are known as methods. Methods are named blocks of code that belong to a class that can be called to execute the code contained within. Let's first take a look at how to declare a method.
###Method Declaration

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