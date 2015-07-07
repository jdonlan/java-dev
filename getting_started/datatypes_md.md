#Java Data Types

At the core of any app are nine basic data types on top of which all functionality is built. Of these nine types, eight of them are what is known as a primitive type; types that aren't "objects". 

These primitive types are defined by the Java language and each one contains its own reserved keyword. Since primitive types aren't considered objects, there is an object type that goes along with each primitive type so that you can use these types with various data collections.  Data collections will be covered in a later section of this book. 

Below are the eight primitive types, along with the ninth type which isn't a primitive type but is often treated like one anyway.

##String
The [String](http://developer.android.com/reference/java/lang/String.html) data type is the one basic data type that isn't considered a primitive type. Strings in Java are actually objects and have their own methods that you can call on  without needing a companion class like with primitive types. The String data type holds a group of characters (any 16-bit Unicode value) to form readable text values. Since strings are objects, they also support holding a null value; a value that specified "no value." Strings are declared using the String class name followed by a group of characters inside of double quotes as seen below.

```String text = "Some Text";```

##Character
A character in Java is any single Unicode value. This usually ends up being an alphanumeric value but can also be any other Unicode value such as common symbols (-, +, =) or letters/characters from other languages. Since characters only support a single Unicode value, use the String type to declare values that require more than one character. A character is declared using the char keyword followed by a single Unicode value inside of single quotes as seen below. To call methods on a character value, use the Character companion class.

```char letter = 'A';```

##Integer
An integer in Java is any whole number value between  negative 2<sup>31</sup> and 2<sup>31</sup>-1, including zero. Integers cannot hold decimal values. If you try to set an integer to be a decimal value then it will be truncated or rounded down. An integer is declared in Java using the int keyword followed by a whole number value. To call methods on an integer value, use the Integer companion class.

```int wholeNumber = 32000;```

##Float
A float is a single-precision floating point, or decimal, number. This data type should be used whenever you need to store a number with a decimal value and only require seven or eight decimal points of accuracy. For just about every Android app, this type should be sufficient for decimal numbers since you're not going to be working with large values very often. If you do need to store a larger number or a number with more decimal places, refer to the double type listed below. To declare a float in Java, you would use the float keyword followed by a decimal number that ends with the letter "f" to signify float. If you leave off the "f" then the compiler will think that your floating point number is actually a double. To call methods on a float value, use the Float companion class.

```float singleFloat = 1.2345f;```

##Boolean
A boolean is a binary (on/off, yes/no, true/false) value that can contain one of two values: true or false. Boolean values are used whenever you need to store data that only has two possible values and those values are opposite. Booleans are also used when performing logic operations such as testing if one is greater than two (1 > 2). This operation would return a boolean value that contains the result which, in this particular case, is false. To declare a boolean, you would use the boolean keyword followed by either a true or false value. To call methods on a boolean value, use the Boolean companion class.

```boolean isJavaAwesome = true;```

##Short Integer
A short integer is just like an integer. The only difference between a short and regular integer is the range of values that are supported. In the case of a short integer, the value must be between -2<sup>15</sup> and 2<sup>15</sup>-1. Since a short holds smaller values than an int, it also uses up less memory. However, the compiler is usually smart enough to replace integers with short integers during compilation so it's much more common to use a regular integer than a short. To declare a short integer in Java, use the short keyword followed by a whole number value. To call methods on a short, use the Short companion class.

```short littleInt = 200;```

##Long Integer
A long integer is also just like a regular integer. The only difference between a long and regular integer is the range of values that are supported. A long integer can support values between -263 and 263-1. Since a long integer holds larger values than a regular integer, it also takes up more memory. Long integers are typically used when dealing with time since time is measure in milliseconds in Java. To declare a long integer, use the long keyword followed by a whole number value that ends with the letter "L" to signify it as a long. Omitting the "L" will signal to the compiler that this is just a regular integer. To call methods on a long, use the Long companion class.

```long bigInt = 1000000000L;```

##Double
A double is a double-precision floating point, or decimal, number. Much like the float data type, doubles can hold decimal values. Since this is a double-precision value, this value can maintain accuracy up to fifteen or sixteen decimal points depending on the size of the whole number value that precedes it. Doubles are typically used when working with formulas where floating point accuracy is tantamount to the formula giving the correct results. If for some reason you need more than sixteen decimal places, you should look into the BigDecimal class. To declare a double in Java, use the double keyword followed by a floating point number. To call methods on a double, use the Double companion class.

```double doubleFloat = 1.123456789;```

##Byte
A byte is a very small integer value between -128 and 127. The byte data type is used to represent the same values of a single byte of memory and is commonly used in file and network operations when reading and writing data. To declare a byte in Java, use the byte keyword followed by a whole number value. To call methods on a byte, use the Byte companion class.

```byte verySmallNumber = 1;```