#Data Collections
For many applications, you'll need to be able to create and manage groups of related objects or types. In this lesson, we'll discuss these related object groups, known as collections, and when to use each within your app.

A group of related objects or types is what's known as a collection. Collections are very commonly used in applications to hold lists of data or key/value pair groupings. Each type of collection has a unique structure and its own set of rules for the type of data it can hold, how much data it can hold, and how that data is accessed. 

Below are four common types of data collections along with the rules for each and best practices for when each type should be used.

##Arrays
Arrays are the simplest form of collection available on the Android platform and are the only collection type actually built into the Java language. Arrays are a finite grouping of one data type. Each item contained within an array is known as an array element and each element is accessed via its numerical index. The length of an array is determined when the array is created and, after that point, the length will never change. You can either create an empty array by specifying a length, or you can create a populated array by specifying the elements contained within.

To access the elements in an array, you must use the zero-based index of the item you want to access. For instance, if you have an array of length 10 and you want to access the first item in the array, you'd use index 0 instead of 1. To access the second item in the array, you'd use index 1 and so on for the length of the array. 

It's good to remember that the max index for an array is always one less than the length.

###Example
```
// Uninitialized array of unknown length
int[] arrayOfInts;
// Empty array of length 10
// Looks like {0,0,0,0,0,0,0,0,0,0}
arrayOfInts = new int[10];
// Accessing the first element in the array
// Looks like {1,0,0,0,0,0,0,0,0,0}
arrayOfInts[0] = 1;
// Accessing the tenth element in the array
// Looks like {1,0,0,0,0,0,0,0,0,2}
arrayOfInts[9] = 2;
// Uninitialized array of unknown length
String[] arrayOfStrings;
// Initialized array of unspecified length but known elements
arrayOfStrings = new String[] {
	"Hello", "New", "Java", "Students"
};
// Getting the length of an array
// Array length here is 4
int stringArrayLength = arrayOfStrings.length;
```

##Lists
Much like with arrays, lists are a type of collection that allows you to store related objects in a sequential fashion. Once objects are added to a list, the ordering of those objects will never change unless you run a sorting routine on them. Where lists differ from arrays is in their flexibility. With arrays, once you declare the size of an array, it never changes. With lists, you can add and remove items from the collection and the size of the list will change based on the number of elements contained within. Also, while it's a best practice to only have one type of object in a list, it's possible for a list to hold multiple objects of different types.

The most common type of list used in Java and Android is known as an ArrayList. ArrayList is the simplest form of list in Java and is used in place of an array whenever you need a more flexible collection. ArrayList supports the adding, removing, and retrieving of objects using the add(), remove(), and get() methods. You can additionally remove all objects in an ArrayList using the clear() method, check to see if the list contains a certain object using the contains() method, and get the index of a particular object using the indexOf() method. 

###Example
```
// Empty ArrayList of String objects
ArrayList<String> stringList = new ArrayList<String>();
// List contains { "Hello" }
stringList.add("Hello");
// List contains { "Hello", "Java" }
stringList.add("Java");
// List contains { "Hello", "Java", "Students" }
stringList.add("Students");
String complete = "";
// Concatenate all strings in the list
for(String string : stringList) {
	complete += string;
}
// complete = "HelloJavaStudents"
// Get the index of the word "Java"
int javaIndex = stringList.indexOf("Java");
// Remove "Java" from the list
stringList.remove(javaIndex);
complete = "";
// Concatenate all strings again
for(String string : stringList) {
	complete += string;
}
// complete = "HelloStudents"
```

Lists and other collections, with the exception of arrays, are what's known as generic classes. Generics classes make use of angle brackets <> and you'll see them more in the collection examples. For more about generic classes, please refer to the next section on Generics.