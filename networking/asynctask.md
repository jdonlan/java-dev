#AsyncTask
As covered in previous sections, to prevent your UI thread from blocking and freezing up your UI, any long running operations should be handled on a separate thread. 

While Android offers several classes for creating custom threads and executing them at different times and in different orders with the java.util.concurrent package, Android also provides a very easy way to perform background operations using the AsyncTask class. AsyncTask is an abstract helper class for easily creating a new thread that takes in some data, performs some defined operations on a background thread, and returns the result of those operations when it finishes.

Remember, operations  performed on a background thread cannot alter UI components as those are handled by the UI thread. To help with this, AsyncTask also has built-in callback methods where you can perform any UI setup you need to do before the task runs, update the UI as the task is running, and update the UI with the returned data after the task finishes. In addition, AsyncTask is a generic class, so you can specify exactly what type of data the task takes in, what data it uses to report progress, and what data the task will return when it finishes.

##Defining an AsyncTask
As mentioned above, AsyncTask is a generic class. While most generic classes will only have one or two types that you can specify, this one has three types as outlined below.

AsyncTask&lt;Params, Progress, Result&gt;
* **Params** - The type to be used for the parameters that are passed into the task when executed. This type is passed into the doInBackground() callback to be used in background operations.
* **Progress** - The type to be used when reporting progress. This is typically an Integer or Long but you can report progress with any type you want.
* **Result** - The type to be used when returning data from the doInBackground() method. This data will be passed to the onPostExecute() callback after doInBackground() returns.

##AsyncTask Callbacks
When creating your own AsyncTask, there are five callbacks that can you can implement but only one of them is required.

###protected void onPreExecute() - Optional
This method is called before doInBackground() starts and is executed on the UI thread. This method allows you to perform any setup that needs to be done before the task starts, such as showing a progress dialog.

###protected Result doInBackground(Params... params) - Required
This is where all the magic happens. This method is executed on a background worker thread and uses the data passed into the execute method to perform background operations. Keep in mind that since this method isn't on the UI thread, you cannot access your UI directly from this method. If you look at the method signature, you'll notice that it takes in the type specified in the Params generic type but you'll also notice that it's qualified with ... after the type. The ... means that any number of parameters can be passed into this method, including none. So if you call execute() with nothing passed in then your doInBackground() won't have anything for the Params... parameter. If you pass in multiple values such as execute(val1, val2, val3), then you can access them in doInBackground() much like you would an array (e.g. params[0], params[1], params[2]).

###protected void onProgressUpdate(Progress... values) - Optional
This method is called whenever you call publishProgress(). This method is executed on the UI thread and is used to update the UI with the task's progress. For example, if you were to create a task to download files, you could publish your progress after each file is downloaded and use this method to update your progress dialog with the number of completed files. As with doInBackground() this method can take in any number of parameters and the parameter type is determined by the Progress type you specified in your class definition.

###protected void onPostExecute(Result result) - Optional/Recommended
Once your doInBackground() method finishes all operations and returns, the data that it returns is passed to this method. This method executes on the UI thread and is meant to update the UI with any data that was updated or retrieved in the doInBackground() method. While this callback is optional to implement, it is highly recommended that this callback is implemented to handle the data returned from the task execution.

###protected void onCancelled(Result result) - Optional
If your task is canceled before it finishes executing, onPostExecute() will never be called but onCancelled() will. The purpose of this method is to handle any data that is returned from doInBackground() if the task is canceled. Sometimes you'll cancel a task just as it finishes and usable data will be returned. If the data is usable, you might want to handle it instead of ignore it which is why this method exists. This method is executed on the UI thread and can be used to update the UI with an error dialog if canceling a task results in an error.

###Inline AsyncTask Implementation
```
// Create a new inline task
new AsyncTask<String, Void, String>() {

	@Override
	protected String doInBackground(String… params) {
		// Grab the parameter
		String parameter = params[0];

		// Return a result;
		return parameter;
	}

	@Override
	protected void onPostExecute(String result) {
		// Do something with the result.
	}

// Executing the task and passing in some data.
}.execute(“Hello”);

```

###Full AsyncTask Implementation
```
public class MyTask extends AsyncTask<String, Integer, String> {

	protected void onPreExecute() {
		// Do setup here.
	}

	protected String doInBackground(String… params) {
		// Do long running operations here.
	}

	protected void onProgressUpdate(Integer… progress) {
		// Update UI with progress here.
	}

	protected void onPostExecute(String result) {
		// Handle the result here.
	}

	protected void onCancelled(String result) {
		// Handle leftover result data here.
	}
}
```

####References
http://developer.android.com/reference/android/os/AsyncTask.html
http://developer.android.com/guide/components/processes-and-threads.html#usingAsyncTask