#File I/O
How do we actually access these files we're storing on the device, what about create new files, and finally store data to and read data from these files? To do this, we use what's known as a stream. Streams are objects that are used to pipe data into or out of a resource. You saw an example of a stream when working with networking and API calls. In that instance, we were using an InputStream to read in data from a remote source.

There are two main types of streams which most other streams inherit from, InputStream and OutputStream. InputStream is used to read in data from a resource while OutputStream is used to write data to a resource. This resource could be a network socket, a byte buffer, or even a simple file. To read and write data with files, we'll use subclasses of the basic stream types, FileInputStream and FileOutputStream. Before we talk about the specifics of how to work with those streams, we must first understand what a file is in Java.

##The File Class
A file in Java is represented by the File class. File objects can be used to represent both files and folders so you'll want to do a few checks on your file object before automatically assuming it's not a folder. Whenever you read or write data with files using file input and output streams, you must first start with a File object. The only exception to that rule is when you work with internal storage, as the system prefers you let the Context or Activity handle the opening of those files.

There are a few operations you can perform with the file class that might be helpful in the future. These operations are: creating a new file, creating a new directory, checking if a file exists, and checking if a file is a directory. We'll start by getting the file that represents the root of our external storage and we'll go through the operations from that point. Let's first get the file representing our external storage directory as shown in the previous activity.

```
File external = Environment.getExternalStorageDirectory();
```

Now that we have a file to work with, the first thing we can demonstrate is how to check if a File object is a file or a directory. In this case, we know we're working with a directory, but we'll still check to show the process. To check if a File is a directory, we can use the isDirectory() method of the File class. This method returns a simple boolean to say whether or not the underlying file is actually a folder.

```
File external = Environment.getExternalStorageDirectory();
if(external.isDirectory()) {
	// external is a folder, not a file
} else {
	// external is not a folder
}
```

Next up, we'll try and make a new folder in our external storage directory. To make a new folder or file, the first thing we do is create a new File object that specifies the path to the file/folder and the name of the file/folder. It should be noted that simply creating a new File object does not actually create a new file/folder. There are specific methods used to actually perform creation. First, we'll check and make sure our folder doesn't already exist using the exists() method. If it does, we'll do nothing. If it doesn't exist, we'll create it using the mkdir() method of the File class. The mkdir() method creates the folder represented by the File object and assumes the parent folders exist. If you're not sure if the parent folders exist, use the mkdirs() method which builds the entire file path to the new folder.

```
File external = Environment.getExternalStorageDirectory();
// Build file object that points to a folder named "newFolder"
// inside the directory specified by the external File object.
File newFolder = new File(external, "newFolder");
if(newFolder.exists()) {
	// Already exists, do nothing
} else {
	// Doesn't exist, make the folder
	newFolder.mkdir();
}
```

The last thing we'll want to do is create a new blank file inside of the new folder we created. Without a stream, it's impossible to create a file that has any content. However, new blank files can be created using only the createNewFile() method of the File class.

```
File external = Environment.getExternalStorageDirectory();
File newFolder = new File(external, "newFolder");
if(!newFolder.exists()) {
	newFolder.mkdir();
}
// Build file object that points to a file named "newFile.txt"
// inside the directory specified by the newFolder File object.
File newFile = new File(newFolder, "newFile.txt");
// If file doesn't exist, make a new one.
if(!newFile.exists()) {
	newFile.createNewFile();
}
```

It should be noted that creating new files in this way is generally not required and not very useful. When writing out to a file using a stream, if the file being pointed to doesn't exist, the stream will create a new file before writing. Creating files in this way is useful though when creating .nomedia files, which are files that have no content. These .nomedia files will be covered later in the book, but just keep in mind that this method of file creation exists as it will be needed down the road.

##FileOutputStream
Now that we know how to work with File objects, let's take a look at how to actually write data out to a file. Of the two operations, input and output, file output is by far the easier of the two. The first thing we need to do is get a File object for where we want to store our data, and some actual data to store.

```
private void writeToFile(Context _c, String _filename, String _data) {
	// Storing in our "protected" directory
	File external = _c.getExternalFilesDir(null);
	File file = new File(external, _filename);
}
```

Now that we have our file, we need to create a new FileOutputStream using that file. Creating a new FileOutputStream has the potential to throw an exception if there are errors in accessing the file, so you have to wrap your code in a try/catch block.

```
private void writeToFile(Context _c, String _filename, String _data) {
	File external = _c.getExternalFilesDir(null);
	File file = new File(external, _filename);
	
	try {
		// Creating new output stream
		FileOutputStream fos = new FileOutputStream(file);
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```

Now all that's left to do is write the data to the output stream and close it all down. OutputStream objects, including FileOutputStream, expect all data to be in a byte[] format. When writing byte or String data this is easy since bytes work natively and the String class has a getBytes() method that returns the string data in byte[] format. Let's write our data to the stream and close it.

```
private void writeToFile(Context _c, String _filename, String _data) {
	File external = _c.getExternalFilesDir(null);
	File file = new File(external, _filename);
	
	try {
		FileOutputStream fos = new FileOutputStream(file);
		// Write bytes to the stream
		fos.write(_data.getBytes());
		// Close the stream to save the file.
		fos.close();
	} catch (IOException e) {
		e.printStackTrace();
	}
}
```

That's it for file output. It's all a matter of opening a new FileOutputStream that points to a file, converting your data to bytes, and writing those bytes out. Once the stream is closed, the file is saved. File input, well that takes a few extra steps.

##FileInputStream
Once you have data that's written to a file, you can load that back into your app using a FileInputStream. Reading in from a file can be done in many different ways, and ultimately it depends on what type of data is stored in your file. If your file contains an image, you'll want to use the BitmapFactory class to load in the data. In this example, we'll cover how to load in string data since it's the most common type of data you'll be working with.

As with before, we'll want to start off with our File object that represents the file we want to read from. We then follow that up by creating a new FileInputStream that points to the file we want to read in. Like with the output stream, creating an input stream also has the ability to throw exceptions so we'll want to wrap it all up in a try/catch block.

```
private String readFromFile(String _filename) {
	File external = getExternalFilesDir(null);
	File file = new File(external, _filename);
	
	try {
		FileInputStream fin = new FileInputStream(file);
	} catch(IOException e) {
		e.printStackTrace();
	}
	
	return null;
}
```

As with FileOutputStream's write() method, the read() method of FileInputStream only works with bytes. To get around needing to work with bytes, we'll instead use stream reader classes to read strings. Stream reader classes like InputStreamReader and BufferedReader are used to read from streams in a safe manner without having to directly access individual bytes. In our example, we'll wrap our FileInputStream in an InputStreamReader to read from the stream. Then we'll wrap our InputStreamReader in a BufferedReader. InputStreamReader is used to read directly from an input stream using character arrays since it can't pull full strings from the stream all at once. BufferedReader is used to wrap other types of readers to allow them to read full strings and full lines of text all at once by buffering several read operations in the background. Since we want to read a lot of text at once, we'll use this method of reading.

```
private String readFromFile(String _filename) {
	File external = getExternalFilesDir(null);
	File file = new File(external, _filename);
	
	try {
		FileInputStream fin = new FileInputStream(file);
		// Creating our stream readers
		InputStreamReader inReader = new InputStreamReader(fin);
		BufferedReader reader = new BufferedReader(inReader);
	} catch(IOException e) {
		e.printStackTrace();
	}
	
	return null;
}
```

Now that we have our stream readers, we can start pulling data from the file using the reader. To do this, we first setup a StringBuffer object. The buffer is used to hold any data we get back from the reader. Then we cycle through each line of text in the file using the reader's readLine() method. The readLine() method returns a full line of text from the underlying stream and strips off the new line character. If you want to keep the new line characters, you'll have to add them in manually as we do in our example. Every time you read a line of text from the reader, add that text to the buffer. When there are no more lines left to read, close the BufferedReader, which will in turn close the underlying stream, and you're done. The StringBuffer now has all of the data as read from the file. Use the toString() method of the StringBuffer class to get the contents as a string, and use it however you wish.

```
private String readFromFile(String _filename) {
	File external = getExternalFilesDir(null);
	File file = new File(external, _filename);
	
	try {
		FileInputStream fin = new FileInputStream(file);
		InputStreamReader inReader = new InputStreamReader(fin);
		BufferedReader reader = new BufferedReader(inReader);
	
		// Reading data from our file using the reader
		// and storing it our string buffer.
		StringBuffer buffer = new StringBuffer();
		String text = null;
		// Make sure a line of text is available to be read.
		while((text = reader.readLine()) != null) {
			buffer.append(text + "\n");
		}
		// Close the reader and underlying stream.
		reader.close();
		// Convert the buffer to a string.
		return buffer.toString();
	} catch(IOException e) {
		e.printStackTrace();
	}
	
	return null;
}
```

####References
http://developer.android.com/reference/java/io/FileOutputStream.html
http://developer.android.com/reference/java/io/FileInputStream.html
http://developer.android.com/reference/java/io/BufferedReader.html
http://developer.android.com/reference/android/os/Environment.html