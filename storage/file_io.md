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