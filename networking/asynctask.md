#AsyncTask
As covered in previous sections, to prevent your UI thread from blocking and freezing up your UI, any long running operations should be handled on a separate thread. 

While Android offers several classes for creating custom threads and executing them at different times and in different orders with the java.util.concurrent package, Android also provides a very easy way to perform background operations using the AsyncTask class. AsyncTask is an abstract helper class for easily creating a new thread that takes in some data, performs some defined operations on a background thread, and returns the result of those operations when it finishes.

Remember, operations  performed on a background thread cannot alter UI components as those are handled by the UI thread. To help with this, AsyncTask also has built-in callback methods where you can perform any UI setup you need to do before the task runs, update the UI as the task is running, and update the UI with the returned data after the task finishes. In addition, AsyncTask is a generic class, so you can specify exactly what type of data the task takes in, what data it uses to report progress, and what data the task will return when it finishes.

##Defining an AsyncTask
As mentioned above, AsyncTask is a generic class. While most generic classes will only have one or two types that you can specify, this one has three types as outlined below.

AsyncTask<Params, Progress, Result>
* **Params** - The type to be used for the parameters that are passed into the task when executed. This type is passed into the doInBackground() callback to be used in background operations.
* **Progress** - The type to be used when reporting progress. This is typically an Integer or Long but you can report progress with any type you want.
* **Result** - The type to be used when returning data from the doInBackground() method. This data will be passed to the onPostExecute() callback after doInBackground() returns.

Sss

