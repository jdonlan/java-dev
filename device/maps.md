#Google Maps

Google Maps is the built-in mapping solution provided by Android and Google Play Services and a big part of making location aware applications that need to display data on a map. In Android, we would accomplish this using the Google Maps API. Google Maps is part of Google Play Services and is available on any Android device that has access to the Google Play Store. That means that, while Google Maps is available on most devices, it's not available on heavily modified versions of Android such as the one developed by Amazon.

##Obtaining an API Key

When developing an app that uses Google Maps, there are several things you must prepare before attempting to display a map. The first thing you need to do is create the project that you want to use Google Maps in and keep track of the package name you used, we'll need that later. The next thing you'll need to do is find the SHA-1 fingerprint of your debug signing key. Every Android application is signed with a debug signing key before it's loaded onto a device or emulator for testing. When you make a release build, you would use a release signing key. If you plan to make a release build, you'll need the SHA-1 fingerprint of that key as well. In order to determine the fingerprint of your signing keys, we'll need to open up a terminal window.

There are a couple of terminal commands we'll need to execute to navigate to our debug key and get the fingerprint of that key. Since most of you are likely not too comfortable with using terminal, we'll walk through each command one at a time at the end. The first thing we'll do is navigate to the folder that holds our debug key. This is a hidden folder in the root of our home directory named ".android". Inside of that folder you should see a bunch of files and/or folders related to your signing key and your emulator. One file should be named either "debug.keystore" or "debug.jks", this is your debug signing key.

![](maps_key_1.png)

Now that we've found the debug key, we need to get the fingerprint of it. To do this, you'll use the Java Keytool utility. This utility is used to both generate signing keys as well as give you information about existing signing keys. We'll run a keytool command on our signing key which will give us both the MD5 and SHA-1 fingerprints of our key. Make sure you're looking at the SHA-1 key and write it down. When running the below command, you will be prompted for a password. The default password for any Android debug key is "android". If that doesn't work, try entering no password as that is also the case at times. Be sure to keep your fingerprint a secret as it will be used to generate API keys later.

![](maps_key_2.png)

The full list of steps and commands to run:

Command - "cd ~/.android" - press return.
Command - "ls" - press return.
Verify that you have a debug.keystore or debug.jks file.
Command - "keytool -list -v -keystore debug.keystore" - press return.
When prompted for password, enter "android" and press return. If that does not work, rerun the above command and enter no password.
Copy down the certificate finger print labeled "SHA1" for future reference.
If you have a release key that you use for assignment submissions, be sure to get the fingerprint for your release key as well. In the terminal window, you would navigate to your release key by typing "cd " and dragging the containing folder into the terminal window and pressing return. Type "ls" and press enter to ensure you can see your release key in the folder. Then, run the command from step four replacing "debug.keystore" with the name of your signing key. The password for your release keystore will be whatever you set it to be when you created the key. If you don't remember the password, you'll have to generate a new key.

Now that we have our package name and our signing key fingerprint(s), we can generate an API key to use for Google Maps. To start, visit [https://code.google.com/apis/console/](https://code.google.com/apis/console/) and login to the Google Account you wish to use for development. Once you login, you should see a large "Create project..." button in the middle of the page. Click that button.

![](google_console.png)

Once you create a project, you'll be taken to a list of available Google APIs. If you're not taken to that screen, click on the APIs & auth tab on the left side of the screen, then click on APIs. In the list of available APIs, scroll down until you find the one that says "Google Maps Android API v2" and turn it on by clicking the "OFF" button on the right side of the screen. The screen may appear slightly different for some people depending on whether or not you've logged into the developer console previously. You will have to accept the terms of service when attempting to enable Google Maps.

![](console_maps.png)

Once Google Maps has been enabled, click on the "Credentials" tab on the left side of the screen. On this page, you'll have the option to create an OAuth client ID or a public API access key. Select the "Create new Key" button under the public API access section. When prompted for a key type, select "Android key". In the resulting dialog, enter your SHA-1 fingerprint, followed by a semi-colon, followed by the package name for your app. Do this all on one line with no spaces. If you have a release key, press return to go to a new line and do the same thing for your release key fingerprint. Click the "Create" button and you should see a new section appear on the page labeled "Key for Android applications" with an "API KEY" that is 30-40 characters long. This is the API key we'll be using in our app.

![](maps_key_3.png)

![](maps_key_4.png)

Now that we have our API key, we can move back over to our application and start setting things up.

##Integrating Google Play Services

The first thing we'll need to do in our app is include the Google Play Services library. To do that, we need to first make sure that we have Google Play services and the Google repository downloaded. If you're unsure whether you've downloaded these or not, open up the SDK manager and download those two items. Next, you need to include Google Play Services as a library dependency. This is done differently depending on your IDE.

![](play_services_sdk.png)

###Google Play Services in Android Studio

In Android Studio, importing Google Play Services is a lot less involved. To add Google Play Services as a dependency, start by opening up your project dependencies by going to File->Project Structure and clicking on the "Dependencies" tab at the top of the window. Click the "+" button in the bottom left corner and select "Library Dependency". Without searching, you should see an entry in the results window named "play-services" (not to be confused with "play-services-wearable"). Select this entry and click OK.

![](play_services_android_studio.png)

##Implementing Google Maps

Now that all of that setup is out of the way, we can finally start working on our app. We'll start by setting up the necessary permissions, features, and meta-data required by Google Maps in our manifest. In order to use Google Maps, you need three permissions for the map and two permissions for location data to go with the map. We'll declare usage of the INTERNET, ACCESS_NETWORK_STATE, WRITE_EXTERNAL_STORAGE, ACCESS_COARSE_LOCATION, and ACCESS_FINE_LOCATION permissions. Additionally, Google Maps uses OpenGL ES v2 to render the map, so we need to declare that with a &lt;uses-feature&gt; tag. Lastly, we'll need to setup two &lt;meta-data&gt; tags inside of our application to declare our Google Play Services version and our Google Maps API key. Our full manifest will look something like this:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.company.android.mapsdemo"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <uses-sdk
        android:minSdkVersion="14"
        android:targetSdkVersion="19" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@android:style/Theme.Holo.Light.DarkActionBar" >
        <activity
            android:name=".MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <meta-data android:name="com.google.android.gms.version" 
            android:value="@integer/google_play_services_version" />

        <meta-data android:name="com.google.android.maps.v2.API_KEY" 
            android:value="YOUR_API_KEY"/>
    </application>
</manifest>
```

Once the manifest is setup, we can start creating our map. To do this, we need to create a subclass of MapFragment. Fragments are an especially important part of building Android applications and Google reinforces this by making their maps available, only by using a MapFragment. There is a MapView object as well, but its use is not recommended with Google Maps v2. When using a MapFragment, be sure not to override onCreateView() as the parent class uses this method to instantiate the map for this fragment. Instead, you should focus entirely on onAttach() for interface communication and onActivityCreated() for any necessary setup. By simply creating this fragment and adding it to our activity, we should be able to view a map on screen. However, this map will be zoomed out and focused on the point where the prime meridian and the equator intersect, or zero degrees latitude and longitude. This isn't very useful, so let's take a look at how we can move this around.

![](default_map_view.png)

By default, the user can pan around your map using swipe gestures, zoom in using pinch gestures, or rotate the map by turning it with two fingers. In addition to those interactions, we can also move the map around using the concept of a camera that can pan left, right, up, and down as well as zoom in and out. To do this, we must first get an instance of our map using the getMap() method of our fragment which returns a GoogleMap object. Then, on our map, we can call animateCamera() and pass in a camera animation. To create a camera animation, we use the CameraUpdateFactory and call any one of the static methods for different camera animations. We'll zoom into Full Sail on the map so we'll use the newLatLngZoom() method which takes in a latitude and longitude, LatLng, object and a zoom level. Seventeen is a fairly high zoom level that should allow you to see the buildings on campus, so we'll start with that.

```
@Override
public void onActivityCreated(Bundle savedInstanceState) {
	super.onActivityCreated(savedInstanceState);
	GoogleMap map = getMap();
	map.animateCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(28.590647, -81.304510), 17));
}
```

Now that we can see the building, let's put a map marker on it. Map markers are the pins that you would see on a map that represent a saved location. All markers have a position represented by a LatLng object as well as a title that shows up when the marker is clicked. Markers are created with a MarkerOptions object and added to the map by calling the addMarker() method of the map. The addMarker() method returns a Marker object that represents the newly added marker. Let's mark Full Sail on the map with a map marker.

```
@Override
public void onActivityCreated(Bundle savedInstanceState) {
	super.onActivityCreated(savedInstanceState);
	GoogleMap map = getMap();
	map.addMarker(new MarkerOptions()
		.position(new LatLng(28.590647,-81.304510))
		.title("MDVBS Faculty Offices"));
	map.animateCamera(CameraUpdateFactory.newLatLngZoom(new LatLng(28.590647, -81.304510), 17));
}
```

![](map_marker.png)

The bit of text that pops up when a marker is clicked is called the info window. By default, the info window shows a TextView with the title of the marker. However, we can customize what this info window looks like by creating an InfoWindowAdapter. The InfoWindowAdapter interface defines two methods, getInfoContents() and getInfoWindow(). The getInfoContents() method defines what shows up inside the speech bubble looking info window. The getInfoWindow() method defines what the window background looks like. You can add an InfoWindowAdapter to a GoogleMap object using the setInfoWindowAdapter() method.

Additionally, you can also capture when the info window is clicked. By default, nothing happens when the info window is clicked, but we can change that using an OnInfoWindowClickListener which defines a simple onInfoWindowClicked() method. The clicked method takes in a map marker which you can then use to identify which marker was clicked.

The last big thing we can do with a map, is get actual map clicks. When the user clicks on a map, we can trap these clicks using an OnMapClickListener and overriding the onMapClick() method. The onMapClick() method, when triggered, will return to you a LatLng object that represents where the user clicked. With this LatLng object you could either pan to that location using the camera or even create a marker to represent that location.

###References
https://developers.google.com/maps/documentation/android/start
https://developer.android.com/reference/com/google/android/gms/maps/package-summary.html

