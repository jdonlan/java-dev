#Thread Types
There are two main types of threads, outside of the main thread: background and worker. 

**Background Threads** are used for long running operations that do some sort of processing but never need to touch the app’s UI. 

**Worker Threads** are used for short running operations that are too long to perform on the main thread, but still need to touch or alter the UI in some way.

While Worker threads can be used to perform operations that update the UI, code that is executed in any thread, other than the main thread, cannot directly touch the application UI. Attempting to alter the UI from other threads might occasionally work, but it might also throw a CalledFromWrongThreadException. As such, an in-between object is needed to communicate with the main thread.

##Runnable
A runnable is a type of object that contains code you want to execute on a thread. Runnable objects have a single method named run() that must be overriden. Runnable objects can be used to execute code on the main thread, but are more often used to run code on a background or worker thread.  When a runnable is executed on a background thread, it is important to note the limitation of background threads not being able to touch the UI.

##Runnable on Background Thread
```
// Create a new Runnable object first.
Runnable r = new Runnable() {
	@Override
	public void run() {
		// Long running operations.
	}
};

// Create a new Thread object using the Runnable.
Thread t = new Thread(r);

// Start the thread.
t.start();
```
##Updating the UI from Thread
While the above example is perfectly fine for performing some sort of background task, such as saving data to storage, without the need to update the UI, many operations will need to touch the UI and thus, need a means to communicate back to the main thread.  There are a few primary mechanisms to doing so...

* Activity.runOnUiThread(Runnable)
* View.post(Runnable)
* Handler.post(Runnable)
* Handler.sendMessage(Message)

###Runnable on UI Thread

Define the Runnable

```
// Create a new Runnable object first.
Runnable r = new Runnable() {
	@Override
	public void run() {
		// Long running operations.
	}
};
```

Option 1 - Calling runOnUiThread(r) will immediately execute the code in the runnable.

```
// From an Activity class.
this.runOnUiThread(r);
```

Option 2 - Posting from an object will add the runnable execution to the end of the event queue, regardless of which thread it is called from.  In this case, the runnable is posted to an ImageView, which is running on the UI Thread.

```
// From a specific view.
ImageView iv = (ImageView)findViewById(R.id.image_view);
iv.post(r);
```

###Runnable with Handler

Handlers are used to process a queue of events that need to happen on the UI thread. The main thread uses a Handler created by the system to process all main thread events.
When using a Handler, they must be declared as static so that they don’t leak memory due to lingering messages in the queue.
Further, Handlers must be created from the main thread in order to be used to update the UI.

When data is needing to be sent to a handler, a Message can be utilized using the Handler's *handleMessage()* method.

####Handler with Post
```
// Creating our static handler.
static Handler handler = new Handler();

// Create the Runnable.
Runnable r = new Runnable() {
	@Override
	public void run() {
		// Long running operations.
	}
};

// Post the runnable to the main thread.
handler.post(r);
```

####Handler using Message
```
// Creating our static handler.
static Handler handler = new Handler() {
	@Override
	public void handleMessage(Message msg) {
		// Message is handled on the main thread.
		MyObject obj = (MyObject)msg.obj;
	}
};

// Create a new message and set an object value.
Message msg = handler.obtainMessage();
msg.obj = new MyObject();

// Send the message to the handler.
msg.sendToTarget();
```

####References
https://developer.android.com/training/multiple-threads/define-runnable.html
https://developer.android.com/training/multiple-threads/communicate-ui.html


