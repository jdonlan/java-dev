#Data Collections
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
arrayOfInts = new int[10]; // Looks like {0,0,0,0,0,0,0,0,0,0}
// Accessing the first element in the array
arrayOfInts[0] = 1; // Looks like {1,0,0,0,0,0,0,0,0,0}
// Accessing the tenth element in the array
arrayOfInts[9] = 2; // Looks like {1,0,0,0,0,0,0,0,0,2}
// Uninitialized array of unknown length
String[] arrayOfStrings;
// Initialized array of unspecified length but known elements
arrayOfStrings = new String[] {
	"Hello", "New", "Java", "Students"
};
// Getting the length of an array
int stringArrayLength = arrayOfStrings.length; // Array length here is 4
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

##Sets
A set is a collection that has a lot in common with list, but is used much less often. Like a list, a set can hold a group of objects in a sequential fashion and you have the flexibility to add and remove objects as well. With a list, however, you can hold duplicate objects while in a set, all objects must be unique. Additionally, the data contained within a set doesn't always maintain the same ordering, and not all sets use sequential ordering of objects.

The most common type of set used in Android is known as a HashSet. A HashSet works in almost the same way as an ArrayList, but with the added restriction of all elements needing to be unique. This means that if you add a thousand random numbers, one to a hundred, to a HashSet then the max size of the set would be 100 and no two elements would be the same. This ends up being very useful if you want to create a collection that works like a list, but you don't want to go through the trouble of looking for duplicates on your own.

Much like with ArrayList, a HashSet supports adding and removing objects from the collection using the add() and remove() methods as well as removing all objects from the set with the clear() method and checking if an object exists within the set using the contains() method. However, with a set, you cannot get a particular object based on index since objects in a set can be stored in sequential order but aren't necessarily stored that way in all sets. 

For instance, the LinkedHashSet class stores objects in the order they were added while a regular HashSet stores objects in a random order.

####Iterators
Since you can't get objects by index, the only way to get objects out of a set is to use an iterator. An iterator is a class that cycles through all objects in a collection and returns them one at a time. You can retrieve this iterator using the iterator() method. Using an iterator to search for a single object is very inefficient since you might have to go through every other element looking for one particular element. This requirement to use an iterator for retrieving set elements makes a set less useful if you need to retrieve objects from your collection. However, if you only need to store items in a flexible manner, have no duplicates, and easily check if an object exists within a collection, then a set is the right collection for the job.

###Example

```
// Empty set of String values.
HashSet<String> stringSet = new HashSet<String>();
// Set now contains { "Hello" }
stringSet.add("Hello");
// Set now contains { "Hello", "Java" }
stringSet.add("Java");
// Set now contains { "Hello", "Java", "Students" }
stringSet.add("Students");
// Set now contains { "Hello", "Java", "Students" }
// Sets will check for duplicate entries and not add repeat values.
stringSet.add("Hello");
// Get the iterator for this set
// Iterator starts at the beginning of the set.
Iterator<String> iter = stringSet.iterator();
// String to hold values from the set
String complete = "";
// Run until we've retrieved all values in the set.
while(iter.hasNext()) {
	// Add the value from the set to the complete string
	// and advance the iterator to the next position.
	complete += iter.next();
}
// Complete = "HelloJavaStudents"
// Ordering of strings varies since HashSet doesn't maintain ordering.
```
While it's required that you cycle through and retrieve objects in a set using an iterator, this functionality is available to all collections, with the exception of arrays and maps, using the iterator() function. 

Arrays have no iterator equivalent, but you can get an iterator for a map by first getting all of the values in a map by calling the values() method. You can then call the iterator() method on the returned values collection and iterate as you would any other collection of data.

##Maps
A map is a very unique type of collection. While arrays, lists, and sets store data in one general grouping, maps store data using key/value pairs. This means that instead of adding a single object to this collection, you must always add two objects; a key object and a value object.

The key object specifies what data to access and the value object is the data that's being accessed. Think of it like a series of locked doors and you have a key ring with all of the keys on it. You can access each of these doors, but to do so, you need the right key. Map data is accessed in very much the same way as the doors. You can't add a value object without specifying a key as well. If you did this, you'd never be able to access the data because you don't know which key opens that door.

The most common type of map used in Android is known as a HashMap. A HashMap is the simplest implementation of a map and allows for the adding, retrieving and removing of key/value pairs using the put(), get(), and remove() methods. You can also check if a certain key or value is contained within the map using the containsKey() and containsValue() methods. 

It's important to note that each key must be unique and you can only have one value associated with each key. However, you can add the same value to a map multiple times, so long as you use a different key each time. Think of it like the group of keys are held in a set (must be unique) and the values are held in a list (not unique). The map is just a way to associate those key objects in the set to the value objects in the list. 

###Example
```
// Empty HashMap using strings for keys and values.
HashMap<String, String> strMap = new HashMap<String, String>();
// Adding 1 with the key "One"
// Looks like { "One"=>"1" }
strMap.put("One", "1");
// Adding 2 with the key "Two"
// Looks like { "One"=>"1", "Two"=>"2" }
strMap.put("Two", "2");
// one = "1"
String one = strMap.get("One");
// Overwriting the key "One" with value 10
// Looks like { "One"=>"10", "Two"=>"2" }
strMap.put("One", "10");
// one = "10"
one = strMap.get("One");
// Removing value associated with key "Two"
// Looks like { "One"=>"10" }
strMap.remove("Two");
// two = null
String two = strMap.get("Two");
```

####References
[http://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)
[http://developer.android.com/reference/java/util/ArrayList.html](http://developer.android.com/reference/java/util/ArrayList.html)
[http://developer.android.com/reference/java/util/HashSet.html](http://developer.android.com/reference/java/util/HashSet.html)
[http://developer.android.com/reference/java/util/HashMap.html](http://developer.android.com/reference/java/util/HashMap.html)
[http://developer.android.com/reference/java/util/Iterator.html](http://developer.android.com/reference/java/util/Iterator.html)
