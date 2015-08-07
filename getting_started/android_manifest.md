#Android Manifest
The manifest file is an essential component of an Android application. In this section, we will go over the data contained in the manifest and how that data relates to the application as a whole.

The Android Manifest file is the one-stop, complete technical overview of your app's components and capabilities. The manifest stores everything from your app's version number, to the type of data it accepts, and much more. Think of it as a table of contents for your application. This manifest file is used by the system to register your app to receive data and to locate app components as they're requested by other application components or even other apps. Additionally, the manifest file is also used by the Google Play store to determine which apps are compatible with different devices, since every Android device has a unique size and resolution, as well as varying sensor and data capabilities.

Below is a listing of common and required components that you would find in the manifest file. The below listing is just a quick look at these components and a more comprehensive list can be found at the App Manifest section of the Google Developer portal.

###&lt;manifest&gt; - required
The manifest tag is the root of all manifest files. This tag is required for all apps and contains key data points such as the app's package name or unique identifier, version name, and version code. The version name follows the standard [major, minor, build, revision](http://en.wikipedia.org/wiki/Software_versioning) naming scheme. The version code however, is merely an integer that's used to identify your app version. Using an integer here makes it very easy for the system and the Google Play store to easily identify which is the most recent version of an app without having to decipher the version name. Everytime you update your app, you update the version code to a higher number. The store and Android system will always assume that the highest version code is the newest or latest version of the app.

###&lt;application&gt; - required
The application tag is contained within the manifest tag and serves as the root for all application components. This tag is required for all applications and contains key information such as the app's name, icon, and theme. Extra attributes are included here to specify system UI options, if the app allows backups, and an option app logo that can be used in place of the icon in certain situations. By default, the system creates a generic application that uses the data defined in this tag when your app starts. However, you can create your own Application class and specify its name as an attribute of this tag. This practice isn't very common, but it's required for certain analytics and crash reporting libraries so it's helpful to keep in mind.

###&lt;uses-sdk /&gt; - recommended
The uses-sdk tag is contained within the manifest tag, typically above the application tag, and is used to define which versions of Android your app supports. To date, there have been over 20 different versions of Android that have been released into the wild. Each version of Android has different capabilities, different UI themes, and different permissions. If you're building an app that uses features of a newer version of Android, you won't be able to support the older versions.

###&lt;uses-permission /&gt; - optional
The uses-permission tag is contained within the manifest tag, typically above the application tag, and is used to define the system permissions your app requires. Certain features of the Android SDK require that your app has permission from the user to take advantage of these features. Having this sort of permission/access system allows the user to see everything that your app is capable of before they install the app, and gives the user the opportunity to decline installation if they don't want your app accessing certain features. 

For instance, the Android SmsManager allows apps to send text messages without user confirmation. To use the SmsManager, your app must request the SEND_SMS permission. If a user is installing an app that uses this permission, they can see up front that the app has the ability to send text messages and determine right then and there if they feel comfortable installing an app with that capability.

###&lt;activity&gt; - recommended
The activity tag is contained within the application tag and is used to define the different activities in your app and contains key data such as the class name of the activity class, the label to be shown on the activity, the icon to be associated with this activity if different from the application icon, as well as any UI options that are specific to this one activity. This tag is not required since not every app has a visual component to it, but almost every single app has some sort of UI that is part of an activity.

There are several other tags that can be found in an app's manifest file which can be found in the Android Developer documentation.

####References
[http://developer.android.com/guide/topics/manifest/manifest-intro.html](http://developer.android.com/guide/topics/manifest/manifest-intro.html)
