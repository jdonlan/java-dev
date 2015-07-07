#Activity Class Overview
Activities are a core building block for almost any Android application. In this section, we will discuss what an activity is, along with how to properly use one in your application.

The Activity class is a core building block of almost every Android application. On the surface, an Android activity is a simple application screen that contains a UI and some functionality that's tied to that UI. Activities are typically full screen, though it is possible to make floating or windowed activities, and the UI on that screen should allow for user input to create an interactive experience. If you dive a little deeper though, activities are so much more than a simple screen with some UI. Activities can respond to system events, connect to content databases, and launch other apps or system components. The more advanced topics will be introduced in a later sections. For now, we'll stick with the simple screen definition of an activity.

##Creating an Activity
If you followed the steps outlined in the project creation lessons for creating a new Android project, you should have an activity in your project named MainActivity. The project creation wizard is a great way to setup your first activity with all the necessary initialization taken care of for you.

The first thing you'll need to do to create a new Activity is to create a new class and have it extend Activity (more on extending classes later in the month). By extending Activity, you're saying that this new class is a type of Activity. You can see how to do this by examining your MainActivity class.

```
import android.app.Activity;
import android.os.Bundle;
public class MainActivity extends Activity {
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	}
}
```

Now that you have a new activity, you need to register it with the application's manifest. Activity is one of the four app building blocks alongside Service, BroadcastReceiver, and ContentProvider for Android. All app building block components must be registered with the Android manifest file so that the system knows how to access them when they're requested by other components or applications. To register the activity in the manifest, you need to create a new &lt;activity&gt; tag inside of the &lt;application&gt; tag. Again, you can see how to do this by looking at the MainActivity entry in the app manifest. Just be certain to change the android:name field to be the fully qualified name of your new activity.

```
<activity
    android:name="com.company.android.myapplication.MainActivity"
    android:label="@string/app_name" >
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

When creating an activity using the project creation wizard, an &lt;intent-filter&gt; tag that contains a MAIN action and a LAUNCHER category will be inserted inside of the activity tag. This specific intent filter is used to signal to the system that this is the main activity in the app and, when the user clicks on the application icon, this is the activity to start. When registering a new activity that is not the main activity, do not include this intent filter. You'll learn more about intent filters and using multiple activities later in this book.

##Activity Lifecycle
Once you click on your application icon and the system starts launching your main activity, your activity goes through a series of lifecycle events. Below is a description of each of these events, when they occur, and what typically happens in each.

###onCreate()
The onCreate() method is the most important lifecycle callback for any activity. This method is called when the system first creates the containing activity and should be used to inflate any layouts or setup any UI components that the user will interact with. By setting up the activity's UI in onCreate, this allows all other methods to act upon UI controls without having to check and see if the UI is available first. This method is always followed by onStart().

###onStart()
onStart() follows onCreate(), and is called when the containing activity becomes visible to the user. Since most of your activity setup is handled there, onStart() only serves niche purposes that benefit from not executing until after the UI is loaded. For example, this method is commonly used for tracking analytics data related to app launching. This method is always followed by a call to onResume().

###onResume()
The onResume() method is called when the containing activity is ready to accept input from the user. At this point, the activity has focus and the user can interact with the activity as you would any other app. This method is typically used for connecting to databases and registering for system events since you'll typically only use those components while the app is running.

###onPause()
The onPause() method is the second most important lifecycle callback method. This method is called whenever the containing activity loses focus but is still visible to the user. At this point, your activity is suspended and doesn't accept user input. When this method is called, you should save any critical data just in case the activity never regains focus. If the activity does regain focus, onResume() will be called again. If the activity doesn't regain focus and instead the app is closed, onStop() will be called.

##onStop()
The onStop() method is called whenever the containing activity is no longer visible to the end user. Typical reasons for no longer being visible include launching a new activity that covers the whole screen, switching apps to bring forward a old activity, or closing the containing app. At this point, the system can decide whether or not to destroy this activity to free up system resources. If the activity restarts without being destroyed, the onRestart() callback will be called. The onRestart() callback isn't covered here since it's hardly ever used and is immediately followed by onStart(), but you can find out more about onRestart() in the Android Developer guide. If the activity doesn't restart and instead it closes completely due to the app being closed or the system freeing up resources, onDestroy() is called.

###onDestroy()
The onDestroy() method is called whenever your activity is being destroyed. This is the last method callback you receive before the containing activity no longer exists. At this point, you should make sure any remaining resources are cleaned up and all data connections are completely closed as to not cause a crash on exit.

####References
[http://developer.android.com/reference/android/app/Activity.html](http://developer.android.com/reference/android/app/Activity.html)
[http://developer.android.com/guide/components/activities.html](http://developer.android.com/guide/components/activities.html)
[http://developer.android.com/training/basics/activity-lifecycle/index.html](http://developer.android.com/training/basics/activity-lifecycle/index.html)

