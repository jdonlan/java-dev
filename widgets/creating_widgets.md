#Creating Widgets
Home screen widgets are a combination of several small components that all come together to form a cohesive widget experience. There's the UI that the user actually sees, the receiver that handles widget update events, the provider info that tells the system how to create the widget, and the configuration activity that's used to allow the user to configure the widget when it's placed on the screen. During the process of creating a widget, there are several minor components that need to be created or updated and it's very easy to miss something. Because of this, we'll go through a complete example from start to finish that shows every step in the process. If you get lost at any part in the process, feel free to compare your project against the provided example to find out what your project might be missing.

Before we begin, it's important to note that widgets are attached to the home screen and execute outside of the scope of your app. This makes widgets incredibly hard to debug. Because of this, be sure to use logcat prints very often to make sure you know where your widget was in execution before it encountered an error.

##Widget Components
When creating a home screen widget, there are a few components that you'll have to create and keep track of. Each component serves a different purpose and all but one of them is required for your widget to work properly. The widget components are listed below.

###Widget UI
This is the actual UI of the widget that users will see when the widget is attached to the home screen. Widgets are positioned on the home screen in a grid so it's important to keep in mind the grid cell sizes when creating your widget UI. Widget size constraints can be found under App Widget Design Guidelines. Another important thing to keep in mind here is that you can't use just any view in your widget UI. The views in your widget are going to be displayed outside of your application. Therefore, we can only use views that contain the @RemoteViews annotation in their class definition. This means that only the following views and layouts can be used in your widget UI: FrameLayout, LinearLayout, RelativeLayout, GridLayout, AnalogClock, Button, Chronometer, ImageButton, ImageView, ProgressBar, TextView, ViewFlipper, ListView, GridView, StackView, and AdapterViewFlipper. This might seem like a large list but notice that common components such as EditText, RadioButton, Spinner, and CheckBox are not included in that list and therefore cannot be part of your widget UI.

###AppWidgetProvider
The AppWidgetProvider is a special type of BroadcastReceiver that is specifically used to send messages to a home screen widget. The AppWidgetProvider class provides several different callback methods, one each for the five different broadcasts that a widget would typically receive. This class is merely provided as a convenience and it's entirely possible to use a regular BroadcastReceiver that handles the ACTION_APPWIDGET_UPDATE, ACTION_APPWIDGET_DELETED, ACTION_APPWIDGET_ENABLED, ACTION_APPWIDGET_DISABLED, and ACTION_APPWIDGET_OPTIONS_CHANGED broadcasts manually. It's more convenient to use AppWidgetProvider so we'll be using that in our demonstration.

###AppWidgetProviderInfo
The provider info is an object that's defined in XML. The provider info defines all of the essential information about a widget that's needed for the system to properly create, update, resize (if possible), and configure your widget. This XML file contains properties such as the widget's minimum size, the interval at which it should be updated, the name of the activity used to configure the widget, how it can resize, the layout for the widget, and where the widget can appear. That last parameter is important since, as of API 17, widgets can appear on either the home screen or the lock screen. In this class, we'll only be focusing on home screen widgets. However, creating lock screen widgets is a very similar process so if you can do one then you can easily do the other.

###Configuration Activity
The configuration activity is an activity that's shown when the widget is first added to the home screen and is used to setup any user preferences for how the widget should be displayed. This component is entirely optional and should only be used if the user should have the ability to configure a widget. If the widget works on its own without any setup, you can omit this activity. For the purposes of this demonstration, we'll go over how this activity can be created.

##Creating the Widget
The first thing we'll need to do, after determining what our widget will actually do, is create the widget's layout. This layout is created exactly the same way as you would create any other layout file. Remember though, you can only use remote view compliant views in your widget layout. For this example, we'll only be creating a very simple widget that shows our app's icon.

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >
    
    <ImageView android:id="@+id/widget_image"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent"
        android:scaleType="centerInside"
        android:src="@drawable/ic_launcher" />
</LinearLayout>
```

For our demo, we're going to have an app configuration activity, so let's go ahead and create the layout and class for that as well. We'll call our configuration activity ConfigActivity and give it a layout with a single button that we'll used to update our widget. For now, we won't register this in the manifest. We'll take care of that a bit later. For now, let's also setup a click listener in our activity for the button.

ConfigActivity Layout:

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical"
    android:gravity="center" >
    
    <Button android:id="@+id/update_button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/update_widget" />
</LinearLayout>
```

ConfigActivity Class:

```
package com.company.android.simplewidgetdemo;
 
public class ConfigActivity extends Activity implements OnClickListener {
 
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_config);
	
		findViewById(R.id.update_button).setOnClickListener(this);
	}
	
	@Override
	public void onClick(View v) {
		
	}
}
```

Now that we have our widget UI and our basic configuration activity setup, we need to create our AppWidgetProviderInfo XML inside of our xml resource folder. The information in this file will define the all of the basic information for our widget. We needed to setup our layout and our config activity first since information about those two components is contained within the widget provider. When setting up your provider info as shown below, you'll notice a warning saying that the widgetCategory value isn't available on pre-API 17 devices. If you add this value in, older devices should just ignore it, so go ahead and suppress that warning if it bothers you to see it.

```
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
    android:minWidth="40dp"
    android:minHeight="40dp"
    android:updatePeriodMillis="30000"
    android:initialLayout="@layout/widget_layout"
    android:configure="com.company.android.simplewidgetdemo.ConfigActivity"
    android:resizeMode="none"
    android:widgetCategory="home_screen" >
</appwidget-provider>
```

If you look at the different properties of our provider info, you can start to see how our widget is going to look and function in a very general sense. Our widget will take up a single grid cell on the home screen (40dp x 40dp) and cannot be resized (resizeMode="none"). Our widget will use the layout we created as its initial layout and our ConfigActivity as the configuration activity. Lastly, our widget will receive an update event every 30 seconds (30,000 milliseconds).

The last thing we need to create is our AppWidgetProvider class. The AppWidgetProvider offers several different receiver methods, one for each standard widget broadcast, but we're going to focus mainly on the onUpdate() method. The onUpdate() method is called once when the widget is first created, then again every time your widget's update interval is met. The only caveat to that is onUpdate() isn't called when the widget is created if you're creating a widget that uses a config activity. If the widget has a config activity, it's the duty of the activity to force the widget to update. It's important to note that onUpdate() is called in very much the same way as onReceive() is called in a BroadcastReceiver. That means that the widget provider will wake up, perform the update, then go back to sleep. If you need to perform any long running operations in order to update the widget, you should start a service to do so.

Inside of our onUpdate() method, we'll do a couple things. The first thing we'll do is determine which widget is being updated. By default, your provider class is used for all instances of your app's widget. If you put five widgets on your home screen, onUpdate() will be called to update all five widgets. The onUpdate() method takes in an array of IDs that describe each widget, make sure you're iterating over that array so that all widgets are properly updated. After we do that, we'll update our widget's layout by setting a click listener to our widget's image. Since this is a remote view and doesn't run inside of our app, we need can't just use a regular click listener. Instead, we need to generate a pending intent and set that pending intent as the action to perform when our image is clicked. For now, we'll just make clicking our widget open up the config activity. Once we do that, we'll simply force our widget to update using the AppWidgetManager class and passing in the widget ID and the updated remote view.

```
public class SampleWidgetProvider extends AppWidgetProvider {
	
	@Override
	public void onUpdate(Context context, AppWidgetManager appWidgetManager, int[] appWidgetIds) {
		
		for(int i = 0; i < appWidgetIds.length; i++) {
			
			int widgetId = appWidgetIds[i];
			
			Intent intent = new Intent(context, ConfigActivity.class);
			intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, widgetId);
			PendingIntent pIntent = PendingIntent.getActivity(context, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);
			
			RemoteViews widgetView = new RemoteViews(context.getPackageName(), R.layout.widget_layout);
			widgetView.setOnClickPendingIntent(R.id.widget_image, pIntent);
			
			appWidgetManager.updateAppWidget(widgetId, widgetView);
		}
	}
}
```

Now that we've created all of our components, let's start hooking them up. The first thing we need to hook up is the AppWidgetProvider and the configuration activity using the manifest. The provider is a subclass of BroadcastReceiver so we'll use the &lt;receiver&gt; tag to take care of that. In the &lt;activity&gt; tag, we'll setup an intent filter to listen for configuration actions while we'll setup our provider to listen for update actions. Also, inside of the provider declaration, we need to setup a &lt;meta-data&gt; tag that tells the system that this receiver belongs to a widget. In that tag, we'll reference the "android.appwidget.provider" property and give it a reference to our provider info XML. Lastly, we'll want to make sure that our configuration activity is hidden from the recent activity viewer. This way it can only be accessed in a proper manner from configuring a widget.

```
<activity android:name=".ConfigActivity"
    android:label="@string/app_name"
    android:excludeFromRecents="true" >
    <intent-filter>
        <action android:name="android.appwidget.action.APPWIDGET_CONFIGURE"/>
    </intent-filter>
</activity>
 
<receiver android:name=".SampleWidgetProvider" >
    <intent-filter>
        <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
    </intent-filter>
    
    <meta-data android:name="android.appwidget.provider"
    	android:resource="@xml/sample_appwidget_info" />
</receiver>
```

The last thing we need to do is finish filling out our configuration activity. There are two important things to keep in mind when working with configuration activities. The first is that you need to know widget you're configuring and the second is that you need to let the system know whether or not the configuration was successful. Everything you do in between, is entirely up to you.

To determine which widget we're updating, we need to get the intent which launched our configuration activity. In the extras of that intent, there should be an integer value defined by the key AppWidgetManager.EXTRA_APPWIDGET_ID. This extra contains the widget ID value. If this value doesn't exist, we have nothing to configure and we should finish the activity. However, we need to let the system know whether or not a widget was updated and, if so, which one. To signal that no widget was updated or that we're canceling the configuration entirely, we would set the result to be RESULT_CANCELED and just finish the activity. However, to tell the system that a widget was updated, we need to create an intent and set the widget ID as an extra using the AppWidgetManager.EXTRA_APPWIDGET_ID key. Then we would set the result to be RESULT_OK and the data to be our updated intent.

For our configuration, we're going to use that simple button we defined and perform the same actions as the onUpdate() method of our widget provider. We do that here since onUpdate() isn't called when the widget is created due to us having a config activity, so we might as well make the widget clickable. Once the widget has been configured, we set our result and close the activity.

```
public class ConfigActivity extends Activity implements OnClickListener {
	
	private int mWidgetId;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_config);
		
		Intent launcherIntent = getIntent();
		Bundle extras = launcherIntent.getExtras();
		
		mWidgetId = AppWidgetManager.INVALID_APPWIDGET_ID;
		
		if(extras != null) {
			mWidgetId = extras.getInt(AppWidgetManager.EXTRA_APPWIDGET_ID, AppWidgetManager.INVALID_APPWIDGET_ID);
		}
		
		if(mWidgetId == AppWidgetManager.INVALID_APPWIDGET_ID) {
			setResult(RESULT_CANCELED);
			finish();
		}
		
		findViewById(R.id.update_button).setOnClickListener(this);
	}
	
	@Override
	public void onClick(View v) {
		updateWidget();
		close();
	}
	
	private void updateWidget() {
		if(mWidgetId != AppWidgetManager.INVALID_APPWIDGET_ID) {
			Intent intent = new Intent(this, ConfigActivity.class);
			intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, mWidgetId);
			PendingIntent pIntent = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT);
			
			RemoteViews widgetView = new RemoteViews(getPackageName(), R.layout.widget_layout);
			widgetView.setOnClickPendingIntent(R.id.widget_image, pIntent);
			
			AppWidgetManager appWidgetManager = AppWidgetManager.getInstance(this);
			appWidgetManager.updateAppWidget(mWidgetId, widgetView);
		}
	}
	
	private void close() {
		Intent result = new Intent();
		result.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, mWidgetId);
		setResult(RESULT_OK, result);
		finish();
	}
}
```

Once you've followed all of the above steps, you should have a working home screen widget that simply shows the app icon and opens up the configuration activity when clicked. This is the absolute simplest widget that you could create in Android. From here, you would change your widget UI to show the data you wanted to show, change your onUpdate() method to update your widget with relevant data, and change your configuration activity to provide the user with actionable options for configuring what's shown in the widget.

####References
https://developer.android.com/guide/topics/appwidgets/index.html
https://developer.android.com/guide/practices/ui_guidelines/widget_design.html
