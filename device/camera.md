#Android Camera

Integrating camera photos into an app is a fairly common task when creating rich media apps. The big draw with apps like Facebook and Instagram is the ability to take a picture and share it with other users. In order to make this type of interaction possible, we need to first understand how to use the camera in our apps to take pictures. Once the picture is in our app, it's a simple ACTION_SEND intent to share the image to other apps or a network POST to send that data to a web server.

There are two ways to integrate the camera into an Android app, internal and external. The external method consists of launching an intent which opens the system camera app and getting the image that was returned. The internal method is much more complicated and involves integrating a camera preview directly into your app and implementing all of your own camera controls and image capturing code. For our apps, we'll focus on the external method. This is because, unless you're making an app where the sole focus is the camera, the internal method is just overkill.

