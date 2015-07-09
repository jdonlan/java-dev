#Storage and I/O
When you want to store data to an Android device, there are numerous locations to do so. Where to store your data depends largely on the type of data you're storing and what kind of access you want other apps having to your data. In this section we'll talk about the different locations in which data can be stored on an Android device and what type of data should be stored in each location.

##Internal Storage
Internal storage is by far the easiest to understand. Internal storage is a private directory located on the app's internal memory unit that can only be accessed by a single app. Each application installed on a given device has their own private, internal storage directory that can be used to store settings, configuration, or any other private data. The data stored in this directory cannot be accessed by any other application nor can it be accessed via a file browser. That limitation goes away, however, if a device is rooted. As such, be sure to not store any highly sensitive data (e.g. username/password values) in this directory unless it's encoded or encrypted in some way. This is a general rule regardless of where you are storing data, but worthy of calling to attention under the lure of the "private" data label.

The types of data you should be storing in this directory should be relatively small. This is not the recommended location to store large files such as images, audio clips, or videos. This location is meant more so to store object or JSON file output that could be used to load private application data. For instance, if you were making a journal app, the list of entries would be stored out to a file in internal storage using either a JSONArray text file or maybe a serialized ArrayList of Entry objects (more on Serializable in a later section). This way other apps can't read the private journal entries created by your application. It's important to note that any files stored in this directory are deleted when the app is uninstalled or if the user clears data from the App Info page in the system settings.

To access files in internal storage, you would use the openFileInput() and openFileOutput() methods of the Context (or Activity) class. These methods return streams which can be used to read from and write to files in internal storage as covered in the following section of this book.

###Example of Internal Storage
```
public class MainActivity extends Activity {
	
	@Override
	protected void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
		setContentView(R.layout.activity_main);
 
		// Read in a private file
		FileInputStream fis = this.openFileInput("some_file.txt");
		// Create new or open existing private file
		FileOutputStream fos = this.openFileOutput("some_other_file.txt", Context.MODE_PRIVATE);
	}
}
```

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

