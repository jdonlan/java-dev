#Data Flow and Extras
When navigating from one activity to the next, it's very likely that you'll want to pass some data to the activity being opened. This can be done using the intent that would already use to launch the activity. Intents have a bundle stored within them called extras. The extras bundle is used to pass data from one activity to the next.

There are two ways to set intent extras, as a full bundle or as separate data points. When using the full bundle, you just create a bundle as you normally would and set it to the intent as shown below.

```
Bundle extras = new Bundle();
 
extras.putString("Name", "John Smith");
extras.putInt("Age", 21);
Intent intent = new Intent(this, NextActivity.class);
intent.putExtras(extras);
startActivityForResult(intent, REQUEST_NEXT);
The other way to set extras for intents is to set the extras directly to the intent and let it store the data in its own internal bundle. You can do this using any of the several putExtra() overload methods that are part of the Intent class.

Intent intent = new Intent(this, NextActivity.class);
 
intent.putExtra("Name", "John Smith");
intent.putExtra("Age", 21);
 
startActivityForResult(intent, REQUEST_NEXT);
```

Now that we've set our intent extras, we need to be able to retrieve them in the newly started activity. To do this, we first need to retrieve the intent that launched the activity. To do this, there is a getIntent() method of the Activity class that will return the intent that launched the activity. Once you have retrieved that intent, you can get the intent extras in the same way that you set them. You can either call getExtras() to get all extras as a bundle, or you can call the different get methods to allow for retrieval of one extra at a time. Since you should be familiar with bundles at this point, we'll show the second way of getting extras.

```
// Inside of the next activity's onCreate() method.
Intent callingIntent = getIntent();
 
String name = callingIntent.getStringExtra("Name");
int age = callingIntent.getIntExtra("Age");
```

The last thing that's important to mention here has to do with how intent extras are named. Having a simple extra name like "Name" or "Age" presents a sort of security or data collision risk. If another app tries to launch your app using simply named extras, they could unintentionally break some of your functionality. So if you expect "Name" to be passed in and it be a string and another app passes in "Name" and it's an integer (for some reason) then you could have a crash. To prevent this naming collision, you should precede all extra names with the package name. So if your extra is named "Name" and your package is named "com.fullsail.android" then your extra name would be "com.fullsail.android.Name" instead. This makes it harder to have name collisions since no other app can share a package name with your app, so it wouldn't make sense for other apps to name variables using your app's package name.

Another good practice is to make these extra names as constant values in the receiving activity. That way you have a way to refer to these values without worrying about getting the exact string name correct each time. Be sure to make them public and static so that you can refer to them from other activities.

```
public class NextActivity extends Activity {
	
	public static final String EXTRA_STRING_NAME = 
		"com.company.android.EXTRA_STRING_NAME";
	public static final String EXTRA_INT_AGE =
		"com.company.android.EXTRA_INT_AGE";
	
	@Override
	protected void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
	
		Intent callingIntent = getIntent();
	
		String name = callingIntent.getStringExtra(EXTRA_STRING_NAME);
		int age = callingIntent.getIntExtra(EXTRA_INT_AGE);
	}
}
```

####References
http://developer.android.com/reference/android/content/Intent.html