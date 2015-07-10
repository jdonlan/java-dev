#Raw Resources
For any resource that doesn't fit into one of the defined Android resource folders, you can instead use the raw resource directory.

When working with rich media apps, it's common to have to provide audio and video assets to be used in your app. These types of files don't fit into a defined resource type, such as layout or drawable, you instead have two options for how to include them in your project. The first option is to use the assets folder. The assets folder allows you to add resource files to your app of any type and access them by name using streams. These files are very easy to access for simple input, but they don't play nicely with the audio and video playback classes. The other option, and the one we'll be using, is to place your files in the "raw" resource directory.

Files placed in the raw resource directory have the same restrictions as files in other resource folders. This means no capital letters, no special characters, and no spaces in the names. This is because Android generates an integer ID to match the name of each file in the raw folder for easy access. Once your files are in the proper directory and named appropriately, there are two ways in which we can access those files.

The first way to access raw resources is using the Resources object. From any context, you can get your app's resources by calling getResources(). The returned Resources object allows you to access any of your app's resources using the corresponding resource identifier. To access a raw resource, you'd use the openRawResource() method of the Resources class. This method works slightly different from the other resource fetching methods. Most resource methods, such as getString() or getDrawable(), simply return the object that represents the resource. However, since raw resources can be any type of file, openRawResource() returns an InputStream which can be used to read in the data from the file as needed.  InputStreams will be covered in a later chapter, but for now, think of them as a data stream of the resources content.

Accessing raw resources through the Resources object works well enough for binary or specially formatted text files, but there's another way to access raw resources that works better for audio and video. Each resource in the res directory can be accessed directly via a special URI. Android defines a special "android.resource://" schema that can be used to directly access resource files in the res directory via URI. When trying to access a resource, the URI is built with the schema, the package name of the app, the resource type, and the resource name minus the file extension. Alternatively, you can also use the schema, the package name, and the resource identifier. The following example shows both URI types for accessing a file named "demo.mp4" from the raw resource directory using a URI.

```
String uri1 = "android.resource://" + getPackageName() + "/raw/demo";
 
String uri2 = "android.resource://" + getPackageName() + "/" + R.raw.demo;
```

It's important to note that there is a key limitation in using this URI for resources. Most file URIs are easily accessible from any app that can read in data. However, resource URIs point directly to private resources within your app. This means that only your app has access to files described by this URI. If you want other apps to have access to your resource files, you'll first need to copy those resources to an accessible directory, like external storage, and pass along the URI to the copied file instead of the private one.

####References
http://developer.android.com/guide/appendix/media-formats.html