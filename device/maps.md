#Google Maps

Google Maps is the built-in mapping solution provided by Android and Google Play Services and a big part of making location aware applications that need to display data on a map. In Android, we would accomplish this using the Google Maps API. Google Maps is part of Google Play Services and is available on any Android device that has access to the Google Play Store. That means that, while Google Maps is available on most devices, it's not available on heavily modified versions of Android such as the one developed by Amazon.

##Obtaining an API Key

When developing an app that uses Google Maps, there are several things you must prepare before attempting to display a map. The first thing you need to do is create the project that you want to use Google Maps in and keep track of the package name you used, we'll need that later. The next thing you'll need to do is find the SHA-1 fingerprint of your debug signing key. Every Android application is signed with a debug signing key before it's loaded onto a device or emulator for testing. When you make a release build, you would use a release signing key. If you plan to make a release build, you'll need the SHA-1 fingerprint of that key as well. In order to determine the fingerprint of your signing keys, we'll need to open up a terminal window.

There are a couple of terminal commands we'll need to execute to navigate to our debug key and get the fingerprint of that key. Since most of you are likely not too comfortable with using terminal, we'll walk through each command one at a time at the end. The first thing we'll do is navigate to the folder that holds our debug key. This is a hidden folder in the root of our home directory named ".android". Inside of that folder you should see a bunch of files and/or folders related to your signing key and your emulator. One file should be named either "debug.keystore" or "debug.jks", this is your debug signing key.

![](maps_key_2.png)

Now that we've found the debug key, we need to get the fingerprint of it. To do this, you'll use the Java Keytool utility. This utility is used to both generate signing keys as well as give you information about existing signing keys. We'll run a keytool command on our signing key which will give us both the MD5 and SHA-1 fingerprints of our key. Make sure you're looking at the SHA-1 key and write it down. When running the below command, you will be prompted for a password. The default password for any Android debug key is "android". If that doesn't work, try entering no password as that is also the case at times. Be sure to keep your fingerprint a secret as it will be used to generate API keys later.

