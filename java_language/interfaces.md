#Interfaces
Interfaces in Java define communication between classes. Interfaces have constructors, methods, and fields similar to those of Java classes, with one significant difference... Unlike classes, where these blocks contain code that define content and behavior - constructors, methods, and fields within an interface serve only as a defined structure for classes that implement them. An interface can thus be considered a contract of structure to which implementing classes must adhere. As these implementing classes are intended to define the actual behaviors and content, interfaces are constructed as follows:

All Fields are Constants
Methods are Signatures Only (no code body)
Why define methods in an interface? Methods defined in an interface contain complete method signatures, which enforce the overall structure of the method. This allows the interface to set expectations which must be followed and allows the application to rely on these expectations to be met. These expectations include the method names, the return type, as well as the parameters and parameter types. This means that any class that implements the interface, must contain a method that follows that exact signature, accepting the same parameters, and returning the same type of data.

Declaring an interface is similar to declaring a class. As with classes, interfaces should be named using a capital letter. Interface fields are constants and should be named using the constant convention of all-caps. Interface methods are declared without a code body, and thus should be defined with a return type, name, and parameter list, then be immediately terminated.

###Example
```
public interface Rules{
  //CONSTANT DECLARATIONS
  public final String INTERFACETAG = "RULES";
  //METHOD SIGNATURES
  public void doSomething(String input);
}
```