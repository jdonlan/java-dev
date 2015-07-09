#URLConnection
As mentioned previously, hundreds of thousands of applications use data connections to connect to internet or network resources everyday. The most common function of these data connections is to pull down text or image data from a web resource or API. To do this, Android provides several different methods for creating a network connection and pulling resources. The only connection method that still gets regular updates in the SDK, however, is the URLConnection class, so we'll focus on this method.

##GET versus POST
Before we get into how to use the URLConnection class, it's important that you first understand the two types of network transactions that you'll be using: GET and POST. A POST transaction does exactly what it says it does, it posts data to a network resource. This type of transaction is commonly used when filling out form data on a web page, such as user registration, where you want to post the data to a web service and add to or alter the data in the corresponding database or datastore. When it comes to mobile apps that use a third party API, you typically won't be making very many post transactions since web services don't want just anybody altering their data.

The other transaction type, GET, is the one you'll be using the most often. GET transactions involve actually getting data back from a web service or API. When performing a GET transaction, you might just hit a static URL endpoint or web address. However, it's very common for GET transactions to accept some sort of query or data parameters attached to the end of the URL and return data that matches the query. It's important to note that GET transactions do not alter the backend database or datastore in the process of returning data.

##Using URLConnection
Now that you understand the differences between GET and POST, let's look at an example of how to use URLConnection to perform a GET call. The first thing we'll need is a URL string. This is the web address that points to the network resource you're trying to access and it looks just like a web page address that you would see in a browser. Once you have your URL string, you'll need to create a new URL object as seen below.

```
// The URL string that points to our web resource.
String urlString = "http://www.example.com/some_api";
// Creating the URL object that points to our web resource.
URL url = new URL(urlString);
```

The URL object is used to create connections to resources that can be accessed via a URL string. That means it can work for HTTP, FTP, file, and other types of resource URLs. The URL class can also be used to parse a URL string into parts and construct a new URL string from parts. Once you've created a new URL, you're ready to create the actual connection.

```
// The URL string that points to our web resource.
String urlString = "http://www.example.com/some_api";
// Creating the URL object that points to our web resource.
URL url = new URL(urlString);
// Establish a connection to the resource at the URL.
URLConnection connection = url.openConnection();
```

After opening the connection between your app and the resource specified by the URL, you're left with a URLConnection object. URLConnection is a class that is used to form connections with and facilitate reading from and writing to network resources. When connecting to an HTTP resource, the transaction type (also known as request method) is set to be GET by default. If you want to change this to POST, you'll have to cast your connection to be an HttpURLConnection. The openConnection() method of a URL object will automatically return an HttpURLConnection if your URL object points to an HTTP resource. However, the object is returned with the type URLConnection to allow for several different types of connections to be supported. We're working with HTTP resources, so we'll cast our connection.

```
// The URL string that points to our web resource.
String urlString = "http://www.example.com/some_api";
// Creating the URL object that points to our web resource.
URL url = new URL(urlString);
// Establish a connection to the resource at the URL.
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
```

Once connected to the network resource, we can set a few different properties that will alter how we interact with that resource. If you wish to change the request method from GET to POST, you can do that using setRequestMethod(). If you want to set your request timeout, the amount of time the connection will wait for a response before giving up, then you can do that with setConnectTimeout(). Likewise, if you want to set the data transfer timeout, the amount of time your connection will attempt to read from a resource before giving up, then use the setReadTimeout() method. There are several more methods available to set different properties, but these are the most commonly used ones. After setting your desired properties, refresh the connection.

```
// The URL string that points to our web resource.
String urlString = "http://www.example.com/some_api";
// Creating the URL object that points to our web resource.
URL url = new URL(urlString);
// Establish a connection to the resource at the URL.
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
// Setting connection properties.
connection.setRequestMethod("GET");
connection.setConnectTimeout(10000); // 10 seconds
connection.setReadTimeout(10000); // 10 seconds
// Refreshing the connection.
connection.connect();
```

At this point we've created a pointer to a network resource, opened the connection, set some request properties, and set our timeout values. We're now ready to start reading from the resource. To do this, we use what's called an InputStream. InputStream is a class that will allow you to read in data from a file or network resource byte by byte. How to use the different capabilities of InputStream is covered in the Storage and I/O chapter. For now, instead, we're going to be using the Apache Commons I/O library to read our InputStream data.

The first thing you'll want to do is get the InputStream that will be used to read from the resource using the getInputStream() method in the URLConnection class. Then, we'll use the IOUtils.toString() method in the Apache library to convert the data in the InputStream into a usable string. After that, we do a bit of clean-up by closing the stream and disconnecting from the resource. Once we've cleaned up, we're done! At this point you will have usable network data pulled from a network resource.

```
// The URL string that points to our web resource.
String urlString = "http://www.example.com/some_api";
// Creating the URL object that points to our web resource.
URL url = new URL(urlString);
// Establish a connection to the resource at the URL.
HttpURLConnection connection = (HttpURLConnection) url.openConnection();
// Setting connection properties.
connection.setRequestMethod("GET");
connection.setConnectTimeout(10000); // 10 seconds
connection.setReadTimeout(10000); // 10 seconds
// Refreshing the connection.
connection.connect();
// Optionally check the status code. Status 200 means everything went OK.
int statusCode = connection.getResponseCode();
// Getting the InputStream with the data from our resource.
InputStream stream = connection.getInputStream();
// Reading data from the InputStream using the Apache library.
String resourceData = IOUtils.toString(stream);
// Cleaning up our connection resources.
stream.close();
connection.disconnect();
// The resourceData string should now have our data.
```

It's important to note that almost all of the methods listed above are capable of throwing an exception, particularly IOException. When working with network connections, be sure to wrap your code in a try/catch block or it won't compile. It's also important to note that URLs don't work with certain characters, including spaces. If you're having trouble with your URL string connecting properly, try using the URLEncoder class to encode your string.

####References
http://developer.android.com/reference/java/net/URL.html
http://developer.android.com/reference/java/net/URLConnection.html
http://developer.android.com/reference/java/net/HttpURLConnection.html
http://commons.apache.org/proper/commons-io/javadocs/api-release/org/apache/commons/io/IOUtils.html
http://developer.android.com/reference/java/net/URLEncoder.html
