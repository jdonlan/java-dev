#Pending Intents

At times, you'll need need to allow other applications to perform actions on behalf of your app. To do this, you would need to give the other app a PendingIntent.

When firing off an intent in your app, that intent is launched with all of the same permissions and access rights as your app. If you were to hand off that intent to another app and allow that app to launch that same intent, it would be launched with the permissions and access rights of the other app. There will be times, however, when you want to hand off an intent to another app and allow that app to launch the intent on behalf of your app, with the permissions of your app. 

For example, when your app creates a notification, you might want that notification to launch your app again when clicked. However, while notifications are created by your app, they're not actually a part of your app, they're part of the Android launcher app. That means that your notification can't execute intents on behalf of your app just by giving it an intent since the launcher doesn't have the same access rights that your app does. Instead, we need to use a PendingIntent.

A pending intent is a holder for an intent and a target action. The intent that you place inside of a pending intent is exactly the same as any other intent that you'd create within your app. The only difference here is that pending intents allow other apps to launch intents using the same permissions and access rights as the app that created the pending intent. Therefore, if you want your notification to launch your main activity, you'd setup an intent to launch that activity in very much the same way as you would any other intent. Then, you'd wrap that intent inside of a pending intent, and set it to be the action on the notification. Then, the notification can use that pending intent and launch your main activity as if the intent was launched from within your app.

##Using PendingIntent
There are two types of pending intents that you'll be using throughout this class and most of your Android development, activity and broadcast. Activity pending intents allow the receiving app to start an activity from your app as if you called startActivity() from within your own app. This is helpful if you want a notification or other system component to be able to start an activity defined in your app. This is also the type of pending intent that we'll use to set the default actions of a notification. To create an activity pending intent, you'd use the static getActivity() method of the PendingIntent class. To call getActivity(), you must pass in a context, an optional request code to identify where the intent came from, the intent that you want the other app to launch, and some optional flags. An example of what this looks like can be seen below.

```
private static final int REQUEST_NOTIFY_LAUNCH = 0x02001;
...
NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
 
Intent intent = new Intent(this, MainActivity.class);
PendingIntent pIntent = 
	PendingIntent.getActivity(this, REQUEST_NOTIFY_LAUNCH, intent, 0);
 
// Setting the default action.
builder.setContentIntent(pIntent);
```

The other type of pending intent, broadcast intents, allows other apps to send a broadcast message on behalf of your app as if a context from within your app called sendBroadcast(). Broadcasts will make a bit more sense once we cover broadcast receivers, but for now think of them as sending a message to the system, in the form of an intent, and any component listening for that message can respond. This type of pending intent is typically used for secondary actions on notifications and your app must define a BroadcastReceiver with a corresponding IntentFilter in order to handle this type of intent. The implementation is very much the same as the activity pending intent, only using a different method in order to create said pending intent.

```
private static final int REQUEST_NOTIFY_BROADCAST = 0x02001;
...
NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
 
// Send an ACTION_SEND intent to whoever can handle it.
Intent intent = new Intent(Intent.ACTION_SEND);
PendingIntent pIntent = 
	PendingIntent.getBroadcast(this, REQUEST_NOTIFY_BROADCAST, intent, 0);
 
// Setting a secondary action.
builder.addAction(android.R.drawable.ic_menu_send, "Send", pIntent);
```

####References
http://developer.android.com/reference/android/app/PendingIntent.html