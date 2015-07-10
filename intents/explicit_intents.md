#Explicit Intents
Explicit Intents are named as such due to their nature of being explicit. When using an Explicit Intent, the application utilizes a fully qualified class name to target and request another component within an application. They are often used to launch activities, services, and broadcasts within the same Android application. Think of it like using a phone number to call specific person. You specify the number to dial, hit dial, and the telephone network connects the call. Similarly, an Explicit Intent informs Android that you are trying to access a specific application component, and Android launches the component, passing any requested data to that component in the process. Passing data between intents is handled through Extras and is covered in the lesson "Data Flow and Intent Extras."

##Building an Intent
It is important to note that both Explicit Intents and Implicit Intents (covered in another lesson) are both similar in how they are executed as they both use Android's Intent class. Explicit Intents are relatively simple to construct in their basic implementation used for requesting the start of a new Activity within an application.

```
Intent nextActivity = new Intent(this, NextActivity.class);
```

In the above example, you can see that the Intent is instantiated using one of the available constructors for the Intent class. In this case, the constructor takes in 2 parameters - the Activity Context, and the target component's class respectively. In this example, the target component is another class within the same application context, and is called out by its class name, followed by the .class designator.

###Triggering an Intent
Once an intent has been established, it must be triggered for Android to actually process the request and launch the corresponding component. This is done simply by calling one of two predefined methods:

* startActivity()
* startActivityForResult()

As is evident by the method names, the rationale to use one over the other is dependent on the desired flow of the application. If the component being launched is intended to return information to the launching Activity, the startActivityForResult() method is used. If the Intent is simply to progress to a new application component without the desire to return data, the startActivity method is used.

###startActivity()
Once the intent is build, starting an activity is as simple as calling the startActivity() method and passing the intent. From there, Android will process the intent, and launch the requested component from the Intent. Using the Intent above, the code would appear as follows:

```
startActivity(nextActivity);
```

Once this code is executed, Android would load the NextActivity.class file, and run the component. As NextActivity extends the Activity class, the onCreate method is executed, proceeding through the normal Activity lifecycle.

###startActivityForResult()
Slightly more complex than simply starting activity is starting one with an expected result. When an Intent is expected to return data, the launching Activity must not only trigger the Intent, but handle the returned data from the launched component. As an Activity could potentially launch multiple Intents, the trigger method uses an additional parameter to identify the result to be returned. For example, taking the above intent, with the expectation that NextActivity would return data, the trigger would look as follows:

```
public static final int NEXT_REQUESTCODE = 1;
```
```
startActivity(nextActivity, NEXT_REQUESTCODE);
```

The NEXT_REQUESTCODE parameter above is a defined integer value that is unique to the Intent. This becomes important when handling the returned data, as this is done through a single function - *onActivityResult*()

###onActivityResult()
When an Activity launches an Intent with the desire to get data back from the launched component, it will need to incorporate the onActivityResult() method. This method accepts three parameters:

* int requestCode - the identifier from the launching intent
* int resultCode - a integer constant informing the launching activity of a successful or failed data return
* Intent data - the intent returned by the launched component

As there is only a single onActivityResult() method to handle any number of Intents, the requestCode parameter is crucial to ensuring the Activity is handling the returned data properly. Continuing with the above example, if we wanted to handle the data returned from the launched NextActivity.class, our onActivityResult might look as follows:

```
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
  if (resultCode == RESULT_OK && requestCode == NEXT_REQUESTCODE) {
    Bundle result = data.getExtras();
  }
}
```

###Returning Data from Launched Activity

The missing component, is how exactly that data gets returned in the first place. To answer this, we need to look at possible code from the Activity launched by the Intent - in this case, NextActivity.

```
private void finishActivity() {
  Intent returnIntent = new Intent();
  returnIntent.putExtra(MYRETURNSTRING, "value");
  setResult(RESULT_OK, returnIntent);
  finish();
}
```

As you can see, a method, finishActivity would be triggered when the NextActivity Activity was ready to close and return data back to the launching Activity. In this case, an intent is created an a String, MYRETURNSTRING, is set with a value of "value." The Activity then establishes a successful execution using setResult(), with a result code of RESULT_OK, and attaches the Intent. At this point, the Activity finishes and the onActivityResult method from the launching activity, as shown above, is run.

####References
http://developer.android.com/guide/components/intents-filters.html
