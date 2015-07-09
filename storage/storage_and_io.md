#Storage and I/O
When you want to store data to an Android device, there are numerous locations to do so. Where to store your data depends largely on the type of data you're storing and what kind of access you want other apps having to your data. In this section we'll talk about the different locations in which data can be stored on an Android device and what type of data should be stored in each location.



##External Storage
External storage refers to any storage location that isn't internal storage. In older versions of Android, external storage meant you were referring to an SD card that was inserted into the device. However, too many applications needed some sort of non-private space to share/store files to, so Android now emulates an SD card on every single device. Referring to external storage on a modern Android OS, thus, means you're referring to either a physical SD card OR a folder on the device named "sdcard". That said, never refer directly to any file or folder to work with external storage as Android has a specific protocol for access these directories depending on the task at hand.

###Public External Storage
The first type of external storage is the one we mentioned above, the "SD card" of the device. When you plug your Android device into your computer and examine the file system using a Windows computer or Android File Transfer for Mac app, the default folder that's shown is the public external storage directory. This folder, and all folders and files contained within, can be accessed by any app on the device with the READ_EXTERNAL_STORAGE permission. Since this directory is public to all other apps, this is not the place to store critical/private app data such as settings or configuration files.

Public external storage is meant to house files that aren't private, and are large. For instance, if your app is used to take pictures and you don't mind other apps being able to see them, you would store them to a folder you created in this directory. Keep in mind, that since other apps can access this directory, it's entirely possible for other apps to delete this data. You'll want to make sure that your app doesn't rely on any data stored in this directory to function as it's entirely possible it can be deleted between launches. On the other side of that, data stored in this directory is not deleted when you uninstall, or clear data for, an application so make sure you're only storing data here that the user would actually want to keep, or at least clean up your data every so often so that you don't leave too much behind if your app is deleted.

You can access the external storage directory using the Environment.getExternalStorageDirectory() method. Using the returned File object, you can create new folders and files as described in the File I/O lesson.

```
// Getting the external storage directory
File sdcard = Environment.getExternalStorageDirectory();
```

###Protected External Storage
The last type of storage is "protected" external storage. It's called "protected" but there aren't actually any protections in place for your data. Protected external storage is a folder in the public external storage directory that is named to match the package name of your application. Since it's located in external storage, any app could traverse the directory and eventually find your folder. However, since it's named for your package name, app's can't directly access the folder without knowing your package name first. So it's publicly accessible, just not easily accessible.

The reason for this directory even existing has to do with how the regular external storage directory works. When a file is stored to the external storage directory, it isn't cleaned up when an app is uninstalled. However, internal storage isn't the appropriate place to store large files. Therefore, if you have large files that you need to store but you don't want them to stick around after your app is uninstalled, you can store them in this directory. Anything stored in this directory isn't cleaned up when app data is cleared, but it is cleaned up when the app is uninstalled. So any large files or images that you need to store that are critical to app functionality should be stored here.

To access the protected external storage directory, you would use the getExternalFilesDir() method of the Context class. This method accepts a string value for a file type that you can use to create multiple file type folders in your protected directory. If you just want the root of your directory, pass in null. The resulting File object can be used to create new files and folders in your directory as described in the File I/O lesson.