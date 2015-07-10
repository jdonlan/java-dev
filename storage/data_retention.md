#SavedInstanceState
Whenever your app changes orientation, or is destroyed by the system for any other reason other than app closure, your activity and all fragments contained within are destroyed and recreated. However, before they're destroyed, a callback is triggered to allow your app to save whatever it was doing so that it can easily resume when its components are recreated. This process of saving out your data is known as saving instance state.

When the system is about to destroy your activity, the onSaveInstanceState() callback is triggered. This method provides your activity with a Bundle object that can be used to store out any data needed so that the activity can be created exactly as it was before it was destroyed. For example, if you were filling out a form and weren't finished yet, you don't want to lose all of your progress if the activity gets killed. To prevent that, you could store all of the data in the fields to the state bundle to repopulate when the activity is recreated. It's important to note that the Android system will try and recreate views exactly as they were, with all of the data that they contained before they were destroyed. However, if the activity has been destroyed for an extended period of time then it might not be possible for the system to do it alone. This is why we store instance state bundles, to prevent data loss in these situations.

When you store instance state, you want to make sure you have an easy way to set all of your data in the state bundle as well as an easy way to retrieve it. For this you'll want to create some string constants to refer to the keys for your bundle. Below you'll find a sample implementation for what this would look like. In the example, we're assuming that this is inside of an activity class named MainActivity. To save state, we simply override onSaveInstanceState() and save our data to the provided bundle.

```
private static final String SAVE_NAME = "MainActivity.SAVE_NAME";
private static final String SAVE_AGE = "MainActivity.SAVE_AGE";
  
private String mName;
private int mAge;
 
@Override
protected void onSaveInstanceState(Bundle _state) {
	super.onSaveInstanceState(_state);
 
	_state.putString(SAVE_NAME, mName);
	_state.putInt(SAVE_AGE, mAge);
}
```

Now that we've saved our state, we need a way to restore that state. In the onCreate() of our activity we're given a bundle named savedInstanceState that, up until now, we haven't used for anything. This savedInstanceState bundle is the same state bundle that we used in onSaveInstanceState(). So in our onCreate() method, we can check if that bundle is null and, if not, use it to restore our state.

```
private static final String SAVE_NAME = "MainActivity.SAVE_NAME";
private static final String SAVE_AGE = "MainActivity.SAVE_AGE";
  
private String mName;
private int mAge;
 
@Override
public void onCreate(Bundle _savedInstanceState) {
	super.onCreate(_savedInstanceState);
 
	if(_savedInstanceState != null) {
		mName = _savedInstanceState.getString(SAVE_NAME, "");
		mAge = _savedInstanceState.getInt(SAVE_AGE, 0);
	} else {
		// Do your regular setup.
	}
}
```

Even if you're not using the save state bundle, it can still be useful to you. When your app is recreated after being destroyed, your app will attempt to recreate any fragments that were a part of your activity. That means that you don't need to recreate them yourself everytime your app is recreated. You can do a simple check in the onCreate() method of your activity to see if you need to add your fragments again.

```
@Override
public void onCreate(Bundle _savedInstanceState) {
	super.onCreate(_savedInstanceState);
 
	// If no state bundle exists, this is the first launch.
	// Add your fragments just this one time.
	if(_savedInstanceState == null) {
		MyFragment frag = MyFragment.newInstance();
		getFragmentManager().beginTransaction().replace(
			R.id.container, frag, MyFragment.TAG).commit();
	} else {
		// Not first launch, don't re-add fragments.
	}
}
```

Lastly, from this point on, you should be using fragments to create your applications. To save state when using fragments, you would override the same onSaveInstanceState() method, only within the fragment class this time. Then, instead of restoring state in onCreate(), you instead restore your fragment state in onActivityCreated(). For our MyFragment class that we referenced above, we could save state as shown below.

private static final String SAVE_NAME = "MyFragment.SAVE_NAME";
private static final String SAVE_AGE = "MyFragment.SAVE_AGE";
  
private String mName;
private int mAge;

```
@Override
protected void onSaveInstanceState(Bundle _state) {
	super.onSaveInstanceState(_state);
 
	_state.putString(SAVE_NAME, mName);
	_state.putInt(SAVE_AGE, mAge);
}
 
@Override
public void onActivityCreated(Bundle _savedInstanceState) {
	super.onActivityCreated(_savedInstanceState);
 
	if(_savedInstanceState != null) {
		mName = _savedInstanceState.getString(SAVE_NAME, "");
		mAge = _savedInstanceState.getInt(SAVE_AGE, 0);
	} else {
		// Do your regular setup.
	}
}
```

####References
http://developer.android.com/training/basics/activity-lifecycle/recreating.html
http://developer.android.com/reference/android/app/Activity.html#onSaveInstanceState(android.os.Bundle)
http://developer.android.com/reference/android/app/Fragment.html#onSaveInstanceState(android.os.Bundle)