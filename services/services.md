#Services
Whenever a user closes an application or sends it to the background, the activities in the app stop running. There will be times, though, when you want some functionality to continue running, even when the app is closed. Earlier in this book, we explored how Activities can start threads and tasks to run on background threads. In order to perform any kind of true background operation in this manner, without an activity, we need to use a service.

A service is a core application component that doesn't provide any sort of user interface. Services are used to perform long running operations that are meant to continue running, even after the starting application has closed. For example, if you start playing music in an activity and it closes, the music stops. However, if you start playing music in a service, and the activity on top closes, the music will keep on playing.

##Declaring a Service
To create a service in your app, you need to extend the Service class or one of its subclasses. Once you extend the Service class, you must also override the onBind() method. The onBind() method is used only in bound services, so we can ignore that for now and just have it return false.

```
public class SimpleService extends Service {
 
	@Override
	public IBinder onBind(Intent intent) {
		// Don't allow binding.
		return null;
	}
 
}
```

Because services are a core application component, they also need to be registered inside of the manifest in order to use them, just like activities. You can register a new service in the manifest using the <service> tag and providing the fully qualified name, or the shortcut name if you prefer, of the service. It's also a good idea to say whether or not your service can be exported. Exporting a service allows other applications to utilize your service, provided they know the package name of your Service class. The services we build in this class won't be exported, so go ahead and set this value to false.

```
<service android:name="com.company.android.SimpleService"
	android:exported="false" />
```

##Using Services
To start using a service, there are two things you need to do first. The first thing to do is call startService() and pass in an intent that describes the service you wish to start. This is typically done from an activity but, as you'll learn later in this course, this can also be called from a broadcast receiver. The intent used to start a service is setup just like the intent used to start another activity. It's an explicit intent meant specifically to start the service.

```
// From an activity or other context:
Intent intent = new Intent(this, SimpleService.class);
startService(intent)
```

The second thing to do is implement the onCreate() and onStartCommand() methods in the service. The onCreate() method works just like onCreate() in an activity. This method is called once when the service is first created and is used to handle any setup that needs to be done before the service can start processing commands. The onStartCommand() method is called each time the service is started via a call to startService(). The intent that is passed into this method represents the intent that was used to start the service. It's important to note that a service can be started several times before being stopped. The very first time startService() is called, the service receives a call to onCreate() and onStartCommand(). The next time the service is started via startService(), onStartCommand() is called but onCreate() is not called.

The onStartCommand() method returns an integer value that represents how the system should handle this service getting killed. If the process hosting this service is killed, the START_STICKY constant tells the system to recreate this service in an already started state. Conversely, the START_NOT_STICKY constant tells the system that, if this process is killed and there are no further startService() calls to process, don't bother recreating the service. We'll get more into service processes below.

```
public class SimpleService extends Service {
 
	@Override
	public void onCreate() {
		super.onCreate();
	}
	
	@Override
	public int onStartCommand(Intent intent, int flags, int startId) {
		return Service.START_NOT_STICKY;
	}
 
	@Override
	public IBinder onBind(Intent intent) {
		// Don't allow binding.
		return null;
	}
	
}
 
// In the calling activity:
Intent intent = new Intent(this, SimpleService.class);
startService(intent); // Creates, starts.
startService(intent); // Already created, just starts.
```

Once the service has been started and is running, it will continue to run until the system kills it, or it is explicitly stopped. There are two methods for stopping a service, stopService() and stopSelf(). If your service is started from another component, such as an activity, and it only needs to exist for the life of the activity, you can call stopService() from the activity and pass in an intent that describes the service you want to stop. However, if the service needs to live on after the activity stops, the service must take care of stopping itself whenever it finishes its work. This can be done by calling stopSelf() from within the service. If the service isn't explicitly stopped, the containing process will continue to run in the background until killed by the system, which can have a significant impact on a device's battery life.

```
// Stopping in the activity:
Intent intent = new Intent(this, SimpleService.class);
stopService(intent);
```

##Important Considerations
It's important to note that, like activities, services extend from the Context class and can be used to access system services in very much the same way as activities can. Any system service that an activity can access, so too can a service using the getSystemService() method. Additionally, with services being contexts, that means that they can also directly access app resources using the getResources() method. This allows services to load in resources such as strings for showing toasts or raw resources for playing background audio.

Another very important thing to keep in mind is how threading is handled in regards to a service. Services run "in the background" but they don't actually run on a background thread. As it was mentioned in a previous course, all application code runs on the main thread by default. The same is true for services. If you have a service and an activity running in the same app, at the same time, then they will share the same main thread and any long running operations happening in the service will slow down the activity's UI.

When a service is said to run "in the background," it's meant that they can run without the app being open, not that they run on a separate thread. However, much like activities, you can start background or worker threads from a service. Additionally, there is another type of service we'll talk about this week called an IntentService, which can be used to do background work in a background thread.

Lastly, any service running in the background can be killed by the system at any time. The system targets services that are running in the background, that aren't bound, and have been running for a long time to kill first. For most services, this is fine as the system will restart your service if it's labeled as sticky whenever it has the resources to do so again. When the service is restarted, onStartCommand() will be called again and your service will receive the last intent that started the service again.

If your service absolutely shouldn't be killed by the system at any point, you can declare a service to be running in the foreground. Typical reasons to run a service in the foreground would be if you were playing music, downloading large files, or processing any sort of time sensitive data. Services that run in the foreground are given a higher priority than services that run in the background. To declare a service to run in the foreground, you need to do two things. The first thing you need to do is create a notification that represents the service. Then, you just need to make a call to startForeground() and pass in the notification and an ID for the notification just like you would any other notification using the NotificationManager class. So long as the service is running in the foreground, that notification will be present in the notification drawer as an ongoing (can't be dismissed) notification. Once your service is done performing any critical work and it's safe to kill it if necessary, be sure to stop running in the foreground with a call to stopForeground().

```
// Inside of the service:
private static final int FOREGROUND_NOTIFICATION = 0x01001;
...
NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
builder.setSmallIcon(R.drawable.ic_notification);
builder.setContentTitle("Playing Music");
builder.setContentText("Blah Blah Song Playing");
builder.setAutoCancel(false);
builder.setOngoing(true);
 
startForeground(FOREGROUND_NOTIFICATION, builder.build());
```

####References
http://developer.android.com/guide/components/services.html
http://developer.android.com/reference/android/app/Service.html
