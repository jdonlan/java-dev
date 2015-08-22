#Fragment Activity Communication

##Communicating through Interfaces
While interfaces have a wide array of functionality and purpose for many Java applications in multiple platforms and genres, one of the most common uses in Android development is creating communication channels between Fragments and Activities. As the Activity should control the application flow/navigation, operate and execute external threads, and facilitate data transfer between individual components, Fragments should utilize the Activity for these tasks. This is done through Java Interfaces.

As Fragments are often used for user-facing components, they typically have their own UI. Behaviors such as click listeners, item selectors, and other UI components are thus handled directly within the Fragment. When these interactions need to communicate with other parts of the application, however, an Interface with the main activity allows the Fragment to communicate with other areas of the application. In order to facilitate this, the Fragment defines the required Interface and the Activity implements the interface. This Interface is often referred to as a Listener, as it enables the Activity to "listen" for events from within the Fragment.

For example, a Fragment could contain a UI that allows the user to search for information from a remote API. The Fragment itself would present the user the controls to enter or select the search terms, and then execute the search. Upon the user electing to execute the search, the Fragment would then inform the Activity that a search needs to be performed. The Activity would spin up a thread, such as an AsyncTask, and perform the search. Once the Activity received the data from the API, the Activity would then be able to pass the data to a display fragment, to display the results. The code for this would look something like:

```
public class SearchFragment extends Fragment{
  public interface OnSearchListener {
    public void searchForSomething(String text);
  }
  private OnSearchListener mListener;
	
  @Override
  public void onAttach(Activity activity) {
    super.onAttach(activity);
    if(activity instanceof OnSearchListener) {
      mListener = (OnSearchListener) activity;
    } else {
      throw new IllegalArgumentException("Containing activity must implement OnSearchListener interface");
    }
  }
}
```

```
public class MainActivity extends Activity implements OnSearchListener{
  //ACTIVITY METHODS
  ...
  
  //ONSEARCHLISTENER METHODS
  public void searchForSomething(String term){
    //ASYNCTASK CODE THAT USES term HERE
  }
}
```

In the above example, you can see that the interface is defined in the Fragment and then implemented in the Activity. As defined in the Interface, the implemented method is public, returns void, and accepts a String parameter.

###Enforcing the Interface
It should be noted that in the above example, there is some logic established in the onAttach method for the Fragment. This logic verifies that the Activity that is attempting to use the fragment implements the proper Interface - OnSearchListener in this case. If the Activity did not implement the interface, an exception would be thrown as the functionality and integrity of the application would be compromised. As the Activity did, however, implement the proper Interface, the Activity context is stored in a member reference mListener. This way, the Fragment can refer back to the containing Activity, allowing the Fragment to communicate with the Activity as needed.

##Communicating Through Bundles
Similar to Activities, in the Fragment lifecycle, the Fragment will go through a series of methods while being attached to the Activity and eventually displayed on the UI thread of the Activity. The onActivityCreated() method provides the opportunity to access and react to data stored in the savedInstanceState Bundle. This is particularly useful for loading default data or restoring data that was loaded into the Fragment previously. Two methods are primarily utilized to store data within the Fragment:

* **getArguments()** - Accesses Fragment Bundle to retrieve stored data.
* **setArguments()** - Stores a Bundle for access.

To use setArguments(), you must first define a Bundle to be saved. Remember, a Bundle acts as a key:value store, so you must store data using put() methods such as putInt(key, value). Retrieving data from these Bundles, similarly, uses a get() method such as getAttributes().getInt(key).

Take, for example, the need to reload text into a TextView upon the Activity being recreated due to an orientation change. First, the Fragment would need to store the text currently being displayed into a Bundle. This is first done on Fragment creation:

```
public static DisplayFragment newInstance(String text) {
  DisplayFragment frag = new DisplayFragment();
		
  Bundle args = new Bundle();
  args.putString("display", text);
  frag.setArguments(args);
		
  return frag;
}
```

At this point, the Fragment has created and stored a Bundle containing text passed from the Activity that is implementing it through its constructor. The Fragment will now need to manage its Bundle in the event the text changes programmatically. Since the Fragment arguments have already been established, the Fragment will need to access and change its current arguments to accomplish this.

```
private void setDisplayText(String text){
  Bundle args = getArguments();
  if(args != null && args.containsKey(ARG_TEXT)) {
    args.putString("display",text);
  }
  ((TextView) getView().findViewById(R.id.display)).setText(text);
}
```

Notice how the same key name display is used to override the data. At this point, the Fragment can handle orientation changes by reloading the data input into the Bundle.

```
@Override
public void onActivityCreated(Bundle savedInstanceState) {
  super.onActivityCreated(savedInstanceState);
		
  Bundle args = getArguments();
  if(args != null && args.containsKey(ARG_TEXT)) {
    setDisplayText(args.getString(ARG_TEXT));
  }
}
```

Here, as the Activity is created, the Fragment calls an internal method of setDisplayText() (shown above) to update its display with the text content.
