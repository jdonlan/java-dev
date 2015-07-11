#Fragment Lifecycle
Since fragments are built to be modules that are swapped in and out as needed, they won't always follow the typical lifecycle of an application. Instead, fragments have their own lifecycle events for creating, starting, stopping, and destroying themselves that's loosely tied to containing activity's lifecycle.

![](fragment_lifecycle.png)

###onAttach(Activity)
This method is called once when the fragment has been added to an activity using the fragment manager. The activity passed into this method is the activity that the fragment has been added to. This method is typically used to determine if the containing activity is suitable to host the fragment.

###onCreate(Bundle)
Much like with activities, this method is called to do the initial creation of the fragment. This is where you would setup any member variables and start loading any data that's needed for the fragment. Unlike with activities, you cannot access any views from this method because the UI for the fragment has not been created yet.


### onCreateView(LayoutInflator, ViewGroup, Bundle)
This method is called when it's time to create the view for the fragment. The sole purpose of this method is to create all layouts and views, likely by inflating a layout file, that are used in the fragment and return them so that they can be added to the activity UI structure. This method is called after onCreate() the first time it executes, but can execute in a different order depending on how the fragment is shown. If the fragment is resuming from a suspended state, this method will be called to recreate the fragment's view from the suspended state. More on that in the lifecycle events below.

###onActivityCreated(Bundle)
This method is called once the containing activity has finished executing its own onCreate() method if the fragment was added to the activity during onCreate(). If the fragment is added to the activity after onCreate() has already finished, this method will still be called immediately after onCreateView() is called. This method can be used to alter views and attach data to your views using the data that was setup in onCreate() and the views setup in onCreateView(). It's important that you don't do this setup during onCreateView() as any operations performed in that method can cause your view creation in other classes to take longer than necessary.

###onStart()
This is where lifecycle methods start to tie in a bit closer to the activity lifecycle. The onStart() method is called when the fragment is first made visible to the user, after onStart() is called in the containing activity. If onStart() has already executed in the activity, this method is still called after onActivityCreated() to maintain lifecycle event consistency.

###onResume()
This method does almost the same exact thing as the activity's onResume() method. At this point, the fragment is in the foreground and accepting user interactions. As with onStart(), this method is called after the activity calls onResume() and, if onResume() has already been called in the activity, then this method will always follow onStart() to maintain lifecycle event consistency.

###onPause()
As with an activity, the onPause() method is called when the fragment can still be seen in the app, but is no longer accepting user interaction. This could be because the containing activity has been paused or because a fragment operation is modifying the activity and contained fragments. The onPause() method should always be the first method called when a fragment is being hidden or destroyed as well.

###onStop()
Again, as with the activity, the onStop() method is called when the fragment is no longer visible to the user. This is either because the containing activity has been stopped or the fragment is being hidden or removed via a fragment transaction. This can also occur if the fragment is pushed onto the backstack.

###onDestroyView()
This method is called after onStop() and is used to clean up any lingering resources associated with a fragment's layout. When fragments are no longer visible to the user, the system saves their state so that they can be quickly rebuilt, but the views are destroyed in order to save memory. When this method is called, you can no longer access a fragment's layout and associated views. If the fragment is resumed, onCreateView() is called again followed by all other lifecycle events necessary to bring the fragment back into the foreground. If the fragment is instead removed from the activity completely, or the activity itself finishes, then the fragment will continue through the below lifecycle events to be destroyed.

###onDestroy()
This method is called when the fragment is no longer needed and is about to be destroyed by the system. At this point you should clean up any remaining fragment data and release any connections to system resources entirely.

###onDetach()
This is the last method in the fragment lifecycle and is called to signal that the fragment is no longer associated with an activity. This method is very rarely used and you likely won't ever have to deal with it in your applications.

####References
http://developer.android.com/guide/components/fragments.html