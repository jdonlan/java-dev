#Location Services

When most people think about location services, the first thing that comes to mind is GPS. However, Android devices, much like other smartphones, can determine their location from several different data providers. GPS is the most accurate, but it's also possible to get location data via your cellular network or even your WiFi. Since GPS is the most accurate, that'll be the location provider we'll cover in the most detail, but we'll also cover the minor changes needed use other providers.

##Location Providers

There are three location providers available on the Android platform. Each provider will return location data at different rates and with varying degrees of accuracy. Below is a description of each.

* GPS_PROVIDER - Uses GPS satellites to determine device location. This has the highest accuracy of any location provider but, since it requires a satellite fix, can take a long time to return a location. If the user is indoors, this provider may never return a location. Using this provider requires the ACCESS_FINE_LOCATION permission in the manifest.
* NETWORK_PROVIDER - Uses cellular towers and WiFi access points to determine location using a network look-up. Since this doesn't wait for a satellite fix, this provider can return locations faster and is ideal for determining location indoors. Using this provider requires the ACCESS_COARSE_LOCATION permission in the manifest.
* PASSIVE_PROVIDER - This provider returns a location without actually obtaining a location fix. What happens is, when other apps request location data, this provider will grab those locations and return them to this app as well. This can return GPS or network locations so you have to check which provider each location came from. This provider isn't used in real-time location apps but is very helpful if you do any sort of background location tracking and don't want to kill the user's battery. Using this provider requires the ACCESS_FINE_LOCATION permission in the manifest. The permission for fine access includes permission for coarse location so this way you can access data from either provider.

##Requesting Location Updates

