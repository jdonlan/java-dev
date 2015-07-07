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

