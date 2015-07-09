#Serializable
Using file streams to save data out to files works well enough string data. Need to save out and read back in some JSON, XML, or lines of text? File streams and stream readers are perfect for the job. What if you want to save an entire object or list of objects? You could first convert your object to key/value pairs in JSON and save that out. If you had a list then you could convert each object to JSON and store them all in a JSON array and store that out as a string. However, that's a lot of work just to save and load some objects. Instead, we can use object serialization to make this easier.

Object serialization is the process of saving an entire object out to a file. The process of saving the object is quite easy, however, it doesn't work for all objects. In order for object serialization to work, you need to first have a serializable object.

##Serializable Objects
Serializable objects are just like any other object. However, the class definition of a serializable object must fulfill three requirements in order to make the objects created with that class serializable:

1. The class must implement the Serializable interface.
2. The class must have a private static final long named
3. serialVersionUID that holds a unique identifier for the class.

All members of the class must also be serializable.
Fulfilling the first two requirements of serializable objects is quite simple. The Serializable interface definition contains no methods that need to be implemented. For this, you need only add "implements Serializable" to the end of your class declaration. Adding in the version UID is also easy as you're just declaring a new private static final long at the top of your class, you never need to do anything with it. If you're using Eclipse and your class implements the Serializable interface, Eclipse will prompt you to create this identifier and you can even have Eclipse generate it for you. It's important to note that you should make every version UID the same for every object as this identifier is used to determine if two serializable objects are compatible.