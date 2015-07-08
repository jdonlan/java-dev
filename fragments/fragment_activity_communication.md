#Fragment Activity Communication
While interfaces have a wide array of functionality and purpose for many Java applications in multiple platforms and genres, one of the most common uses in Android development is creating communication channels between Fragments and Activities. As the Activity should control the application flow/navigation, operate and execute external threads, and facilitate data transfer between individual components, Fragments should utilize the Actiivty for these tasks. This is done through Java Interfaces.

As Fragments are often used for user-facing components, they typically have their own UI. Behaviors such as click listeners, item selectors, and other UI components are thus handled directly within the Fragment. When these interactions need to communicate with other parts of the application, however, an Interface with the main activity allows the Fragment to communicate with other areas of the application. In order to faciliate this, the Fragment defines the required Interface and the Activity implements the interface. This Interface is often referred to as a Listener, as it enables the Activity to "listen" for events from within the Fragment.

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

