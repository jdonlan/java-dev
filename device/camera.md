#Android Camera

Integrating camera photos into an app is a fairly common task when creating rich media apps. The big draw with apps like Facebook and Instagram is the ability to take a picture and share it with other users. In order to make this type of interaction possible, we need to first understand how to use the camera in our apps to take pictures. Once the picture is in our app, it's a simple ACTION_SEND intent to share the image to other apps or a network POST to send that data to a web server.

There are two ways to integrate the camera into an Android app, internal and external. The external method consists of launching an intent which opens the system camera app and getting the image that was returned. The internal method is much more complicated and involves integrating a camera preview directly into your app and implementing all of your own camera controls and image capturing code. For our apps, we'll focus on the external method. This is because, unless you're making an app where the sole focus is the camera, the internal method is just overkill.

##Implementation

The very first thing we need to do is tell the system what our app does and what capabilities are required. We'll be storing our images in external storage, so we'll need to request the READ_EXTERNAL_STORAGE and WRITE_EXTERNAL_STORAGE permissions in order for our saving/retrieving to work. Our app is also going to require the use of the camera. While it might seem like a given that all devices will have a camera, that's not entirely true. Android is being installed in TVs, cars, computers, and even game consoles. Most of those devices aren't going to have a camera available. In order to tell the system that our app needs a camera, or else it can't install, we use the &lt;uses-feature&gt; tag in the manifest. This goes inside the &lt;manifest&gt; tag, but outside the &lt;application&gt; tag.

```
<uses-feature android:name="android.hardware.camera"
    android:required="true" />
```



