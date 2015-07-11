#Location Services

When most people think about location services, the first thing that comes to mind is GPS. However, Android devices, much like other smartphones, can determine their location from several different data providers. GPS is the most accurate, but it's also possible to get location data via your cellular network or even your WiFi. Since GPS is the most accurate, that'll be the location provider we'll cover in the most detail, but we'll also cover the minor changes needed use other providers.

##Location Providers

There are three location providers available on the Android platform. Each provider will return location data at different rates and with varying degrees of accuracy. Below is a description of each.

* GPS_PROVIDER - Uses GPS satellites to determine device location. This has the highest accuracy of any location provider but, since it requires a satellite fix, can take a long time to return a location. If the user is indoors, this provider may never return a location. Using this provider requires the ACCESS_FINE_LOCATION permission in the manifest.
* NETWORK_PROVIDER - Uses cellular towers and WiFi access points to determine location using a network look-up. Since this doesn't wait for a satellite fix, this provider can return locations faster and is ideal for determining location indoors. Using this provider requires the ACCESS_COARSE_LOCATION permission in the manifest.
* PASSIVE_PROVIDER - This provider returns a location without actually obtaining a location fix. What happens is, when other apps request location data, this provider will grab those locations and return them to this app as well. This can return GPS or network locations so you have to check which provider each location came from. This provider isn't used in real-time location apps but is very helpful if you do any sort of background location tracking and don't want to kill the user's battery. Using this provider requires the ACCESS_FINE_LOCATION permission in the manifest. The permission for fine access includes permission for coarse location so this way you can access data from either provider.

##Requesting Location Updates

Each of the different providers have their different use cases, but for our demo we're going to focus on the GPS provider since it offers the highest level of accuracy. To run this demo, you might have to be near a window or even venture out into the dreaded outdoors (the day star burns!) to get a location fix.

With that in mind, the first thing we'll do is grab the system service for handling location updates, LOCATION_SERVICE. The location service request, like other system service requests will return an Object that we must cast to the appropriate type in order to use it. In our case, the location service returns an object of type LocationManager.

```
// Assuming in a Context.
LocationManager mgr = (LocationManager)getSystemService(LOCATION_SERVICE);
```

Now that we have our manager, we'll need to implement the LocationListener interface in order to start requesting location data from the location manager. The LocationListener interface has four methods that need to be implemented, but we're only going to focus on one of them for now. For your information though, here's what each method does:

###onLocationChanged(Location location)
This is the method that we'll be focusing on in our demo. This method is called at a semi-regular interval while the location manager is requesting updates. The location that is passed into this method is the current location of the device as determined by the provider used to start requesting updates. If you're using the passive provider, you can always call getProvider() on the location to find out which provider returned the actual location data.

###onProviderDisabled(String provider)
This method is called whenever the user disables a provider that is requesting updates. If the app starts requesting updates and the provider is disabled to start with, then this method is called immediately after requesting updates. The name of the disabled provider is passed into this method so that you can either switch to a different provider or inform the user that they need to re-enable the provider to get location data.

###onProviderEnabled(String provider)
This method is called whenever a provider that was previously disabled is enabled by the user. This method is only called if the user enables a provider after it has started requesting updates. That means, unlike onProviderDisabled(), this method is not called immediately if the provider is enabled when starting to request updates.

###onStatusChanged(String provider, int status, Bundle extras)
This method is used to check the status of a given provider. This method would be called if your provider is unable to fetch location data for some reason, such as being out of service for network providers, or if the provider has become available after a period of being unavailable.

For our listener, we're going to focus entirely on the onLocationChanged() method. In that method, we care about two main things, the latitude and the longitude. At the very core of location data, you're working with a single geographical point determined by latitude and longitude. The Location object returned can be used to determine additional things like the location accuracy, the bearing, the elevation level, and the distance between two Location points. However, all we're going to focus on right now is the lat/long as that's what you'll need when you work with 99% of location APIs.

```
@Override
public void onLocationChanged(Location location) {
	double latitude = location.getLatitude();
	double longitude = location.getLongitude();
}
```

Once we've got our listener setup, we can start requesting location updates from the LocationManager. When requesting updates, you'll need to provide four things. The first is the name of the provider you wish to use. In our case, we'll supply GPS_PROVIDER to get data from the GPS. You'll also need to supply a time in milliseconds that represents how often your app should receive location data. If you're building a navigation app, you might want to have this set at something like 1000ms or one second. However, requesting updates too often can kill your battery life so, for non-navigation apps, you might want to set your interval at something slower like 5000ms or five seconds. The next parameter you need to supply is the distance factor by which you want to request updates in meters. If you set this at 10 meters, you'd only receive updates if your location has changed more than 10 meters since the last update. Lastly, you'll supply your listener.

```
mgr.requestLocationUpdates(LocationManager.GPS_PROVIDER, 5000, 10, this);
```

That's it for requesting updates. At this point, your app should be requesting location updates at a five second interval after moving at least 10 meters, or at least if the device thinks it moves 10 meters. If you need persistent location data, go ahead and leave your updates running. However, in most cases you only need to get the location once. In that case, be sure to stop requesting updates after you've received a location update. In some cases, you might not need to request updates at all. Sometimes, your device will have a last saved location available for you to access. If you only need a single location, try requesting that location before attempting to request location updates.

```
Location location = mgr.getLastKnownLocation(LocationManager.GPS_PROVIDER);
if(location == null) {
	// No location available.
}

```

####References
https://developer.android.com/guide/topics/location/strategies.html
https://developer.android.com/reference/android/location/LocationManager.html
