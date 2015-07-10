#IntentService
While most services share a main thread with other application components, there is a type of service that can be used to spawn worker threads to handle all operations.

As all services share a main thread with other application components by default, any intense operations performed in a service still need to be offloaded to a background thread as to not slow down the UI in any activities on that main thread. Setting up worker and background threads in a service works exactly the same way as if you were to do this from an activity. You can create threads and execute AsyncTasks, or perform any other threading operation as if you were in an activity. However, there's a type of service that's built specifically to handle operations in a background thread that can take care of all of that work for you.

##Using an IntentService
An IntentService is a special type of service that's used to perform operations in a background thread. When this service is started, instead of creating a service that runs until stopped, an IntentService will start up, do some work in a background thread, then stop itself when it finishes. Let's take a look at how this would work in code.

The first thing we'll do is create a class that extends IntentService. Since this is still a type of service, don't forget to register this in the manifest. Once you create your subclass, you'll have to override the constructor and the onHandleIntent() method. In the constructor, you must call the super constructor and pass in a name for the class. This name is used to identify your worker thread and is immensely helpful when debugging.

```
public class BackgroundService extends IntentService {
 
	public BackgroundService() {
		super("BackgroundService");
	}
 
	@Override
	protected void onHandleIntent(Intent intent) {
 
	}
}
```

When building an IntentService, you don't use the onBind() and onStartCommand() methods that you would use with other services. Those methods execute on the main thread and our goal with this is to not touch the main thread. Instead, the onHandleIntent() method is called from a worker thread. In this method we can do any long running operations and don't have to worry about any sort of slowdown.

In order to start an IntentService, we still create the same sort of intent that we created with any other service and call startService(). The only difference here is that we'll want to pass some sort of extras into that intent to tell onHandleIntent what type of work to do.

```
// From an activity:
Intent intent = new Intent(this, BackgroundService.class);
intent.putExtra("Some key", "Some value");
startService(intent);
```

This simple model works well enough if you don't need to return any data, but let's say that we do. In order to do this, we must create a ResultReceiver class. ResultReceiver is a special type of object that can be passed around across threads in order to update one thread with data from another thread. This makes it a perfect candidate for updating our main thread with data from the worker thread spawned in onHandleIntent().

The first thing we need to do is, in our activity, create a new class that extends ResultReceiver. You'll need to override the constructor and the onReceiveResult() methods. The constructor requires you give it a Handler that was created on the main thread. So, we'll create a Handler in our activity and use that to create the receivers. Now, in the onReceiveResult() method, we can handle any data that's returned from the service. To send this to the service, we can just add an instance of the receiver to the intent as an extra. Then we can fetch this receiver as an extra in the service, cast it to a ResultReceiver NOT THE SUBCLASS TYPE and call send() on the receiver to send back the result. We don't cast this to the subclass type because ResultReceiver is a Parcelable object and we're not defining a CREATOR property in our subclass. That means that the system can only cast this to the base type and not the subclass type. If you try and cast it to the subclass type, your app will throw an exception.

###Example

MainActivity

```
public class MainActivity extends Activity {
	
	public static final String EXTRA_RECEIVER = "MainActivity.EXTRA_RECEIVER";
	public static final String DATA_RETURNED = "MainActivity.DATA_RETURNED";
	
	public static final int RESULT_DATA_RETURNED = 0x0101010;
	
	TextView mResultView;
 
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		mResultView = (TextView)findViewById(R.id.returned_text);
 
		Intent intent = new Intent(this, BackgroundService.class);
		intent.putExtra(EXTRA_RECEIVER, new DataReceiver());
		startService(intent);
	}
	
	private final Handler mHandler = new Handler();
	
	public class DataReceiver extends ResultReceiver {
		public DataReceiver() {
			super(mHandler);
		}
		
		@Override
		protected void onReceiveResult(int resultCode, Bundle resultData) {
			if(resultData != null && resultData.containsKey(DATA_RETURNED)) {
				mResultView.setText(resultData.getString(DATA_RETURNED, ""));
			}
		}
	}
}
```

Background Service

```
public class BackgroundService extends IntentService {
 
	public BackgroundService() {
		super("BackgroundService");
	}
 
	@Override
	protected void onHandleIntent(Intent intent) {
		if(intent.hasExtra(MainActivity.EXTRA_RECEIVER)) {
			ResultReceiver receiver = (ResultReceiver)intent.getParcelableExtra(MainActivity.EXTRA_RECEIVER);
			Bundle result = new Bundle();
			result.putString(MainActivity.DATA_RETURNED, "It works!");
			receiver.send(MainActivity.RESULT_DATA_RETURNED, result);
		}
	}
}
```

##IntentService Considerations
Like with bound services, IntentService has an odd lifecycle. When startService() is called, the service is started. However, when onHandleIntent() is finished executing, the service stops itself. The only caveat to that is if there are more intents to handle. The onHandleIntent() method can only handle one intent at a time. If you call startService() several times in a row, the intents you send will be processed one at a time. When all intents are done processing, then the service will stop itself.

The other big thing to note with an IntentService is that you shouldn't override the onStartCommand() method. The superclass version of this method handles the creation of the worker threads and passing of intents. If you alter this implementation, your service won't work.

####References
https://developer.android.com/training/run-background-service/create-service.html
http://developer.android.com/reference/android/app/IntentService.html
http://developer.android.com/reference/android/os/ResultReceiver.html
