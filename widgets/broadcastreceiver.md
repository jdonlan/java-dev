#BroadcastReceiver
Intents are used to start activities, implicitly launch apps, and start services. Intents, and the extras they contain, can also send messages and pass data between app components through the use of broadcasts. Broadcasts take an intent, with a defined action or category, and sends it to all receivers that are setup to handle that intent type. Those receivers can then process that intent and perform any necessary actions using the data contained within. In order to work with broadcasts, we need to define a receiver or handler of our own.

To define broadcast intent handlers, we need to create a subclass of the BroadcastReceiver class. A BroadcastReceiver is a special type of application component that sits quietly in the background of an app and listens for intents of a specified type to be broadcasted. Once a matching intent is broadcasted, the receiver wakes up and receives a call to the onReceive() method. In this method, the receiver would process the data in the intent, perform any necessary actions, then return. Once that method returns, the receiver goes back to sleep until another matching intent is broadcasted.

##Sending Broadcasts
Before we go too much into how intents are handled by the receivers, let's talk about how to send broadcasts and what type of intents work with broadcasts. To start, there are two types of intent actions, activity and broadcast. Activity intent actions are used to start an activity that performs a specific function. Examples of this would be ACTION_VIEW or ACTION_SEND implicit intents as each of these would open an activity that can handle those intents. Broadcast intent actions are used to send data or information to interested receivers for their own use. These intents don't directly open any app components, they merely send information to components to be consumed. Examples of this would be the ACTION_BOOT_COMPLETE intent that tells apps when the system has finished booting up after powering on and the ACTION_TIME_TICK intent that tells apps that the time has changed. Neither of these intents open any apps, they're just passing along data. You should never try to broadcast an activity intent or start a broadcast intent as the system wouldn't know how to handle them, and nothing would happen. A listing of the standard actions for each intent type can be found in the Intent class documentation.

So we know we can't broadcast standard activity intents, but we can send standard broadcast intents, right? Wrong. Of all the standard broadcast intents, all but one is a protected system intent. That means that only the system can send a broadcast of that type. If your app tries to send a broadcast using a protected action, your app will crash with a SecurityException. So if we can't send activity intents and we can't send broadcast intents, what can we send? Well, you send custom intents.

If you want to send data to other app components using a broadcast intent, you need to define your own custom action first. Then, you would create a new intent using the custom action and broadcast that intent using the sendBroadcast() method of any Android context. Remember that both activities and services are contexts, so both of them can send broadcasts using this method.

```
// Assuming we're in an activity.
public static final String ACTION_CUSTOM =
	"com.fullsail.android.ACTION_CUSTOM";
...
Intent broadcast = new Intent(ACTION_CUSTOM);
sendBroadcast(broadcast);
```

##Handling Broadcasts
Now that we can send a broadcast with our custom action, we need a BroadcastReceiver to handle that intent. The first thing we need to do is define a new subclass of BroadcastReceiver and override the onReceive() method. The onReceive() method is the only required override when implementing your own receiver class and it's only ever called when your class receives an intent that it's listening for.

```
public class CustomReceiver extends BroadcastReceiver {
	@Override
	public void onReceive(Context context, Intent intent) {
		// Intent handled here.
	}
}
```

Now that we've defined our receiver class, we need a way to tell our receiver to start listening for intents of a specific type. To do this, we would use an intent filter. Intent filters are objects that tell app components to only respond to intents of a specified type. As you might recall, the main activity in any app only listens for ACTION_MAIN intents as defined by the intent filter in the app manifest. We can also set intent filters to activities when we listen for implicit intents. Well, we can do the same thing with receivers as well.

There are two ways to define intent filters for receivers and the use case for each varies. The first way is through the app manifest. Receivers and intent filters defined in the app manifest start listening for intents of the specified type as soon as an app is installed. The defined receiver will always listen for intents of that type and it cannot be stopped in any easy fashion. This is helpful if your app always needs to respond to certain system events. For instance, if your app is setup to be a text messaging app then it should always listen for the SMS_DELIVER_ACTION intent which fires every time the device receives a text message. If your app isn't listening for this intent at all times, then it won't get text messages in real time. To define a persistent BroadcastReceiver in the manifest, you'd use the &lt;receiver&gt; tag.

```
<receiver android:name=".CustomReceiver" >
    <intent-filter>
        <action android:name="com.company.android.ACTION_CUSTOM" />
    </intent-filter>
</receiver>
```

It's important to note that, unlike other core app components such as activities and services, not all receivers need to be registered in the manifest. Instead of registering a receiver in the manifest for persistent listening, you could instead do this during runtime to only listen for broadcasts that happen while your app is open. To do this, we need to first setup our intent filter, only this time we'll be using an IntentFilter object instead of a manifest tag. The IntentFilter class allows you to create a new filter and add any actions that you want to filter in your receiver. Then, you can register a receiver to listen for broadcasts by calling registerReceiver() from a context and passing in an instance of the receiver and the filter that you want to apply to the receiver.

Once a receiver is registered in code, it will continue to listen for data even if the service it was attached to is stopped or the activity it's attached to is sent to the background. Because of this, you should always make sure to register your receiver in onResume() and unregister the receiver in onPause() for activities. For services, you would register the receiver in onCreate() and unregister in onDestroy().

```
// Again, assuming we're in an activity.
public static final String ACTION_CUSTOM =
	"com.company.android.ACTION_CUSTOM";
 
CustomReceiver mReceiver;
 
@Override
protected void onResume() {
	super.onResume();
	
	mReceiver = new CustomReceiver();
	
	IntentFilter filter = new IntentFilter();
	filter.addAction(ACTION_CUSTOM);
	registerReceiver(mReceiver, filter);
}
 
@Override
protected void onPause() {
	super.onPause();
	
	unregisterReceiver(mReceiver);
}
```

