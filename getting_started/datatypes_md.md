#Java Data Types

At the core of any app are nine basic data types on top of which all functionality is built. Of these nine types, eight of them are what is known as a primitive type; types that aren't "objects". 

These primitive types are defined by the Java language and each one contains its own reserved keyword. Since primitive types aren't considered objects, there is an object type that goes along with each primitive type so that you can use these types with various data collections.  Data collections will be covered in a later section of this book. 

Below are the eight primitive types, along with the ninth type which isn't a primitive type but is often treated like one anyway.

##String
The String data type is the one basic data type that isn't considered a primitive type. Strings in Java are actually objects and have their own methods that you can call on  without needing a companion class like with primitive types. The String data type holds a group of characters (any 16-bit Unicode value) to form readable text values. Since strings are objects, they also support holding a null value; a value that specified "no value." Strings are declared using the String class name followed by a group of characters inside of double quotes as seen below.

```String text = "Some Text";```

##Character
A character in Java is any single Unicode value. This usually ends up being an alphanumeric value but can also be any other Unicode value such as common symbols (-, +, =) or letters/characters from other languages. Since characters only support a single Unicode value, use the String type to declare values that require more than one character. A character is declared using the char keyword followed by a single Unicode value inside of single quotes as seen below. To call methods on a character value, use the Character companion class.

```char letter = 'A';```

##Integer
An integer in Java is any whole number value between -2<sup>31</sup> and 2<sup>31</sup>-1, including zero. Integers cannot hold decimal values. If you try to set an integer to be a decimal value then it will be truncated or rounded down. An integer is declared in Java using the int keyword followed by a whole number value. To call methods on an integer value, use the Integer companion class.

```int wholeNumber = 32000;```