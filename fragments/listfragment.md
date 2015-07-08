#ListFragment
Displaying a list of data in a ListView is one of the most common tasks in building an Android app. To help make this a bit easier, and to help break lists up into their own module, the Android SDK provides the ListFragment class. List fragments are a special type of fragment that have a list UI and some extra list functionality built in to the class. Whenever you need to build a fragment that contains a list, and only a list, be sure to use this type of fragment instead of the simple Fragment class.

##Creating a ListFragment
When creating a new list fragment, we start off by creating a subclass of the android.app.ListFragment class. Like with regular fragments, make sure you're using this class and not the support library equivalent. For this example, we'll create a list of presidents so let's call our fragment PresidentListFragment. You should always name your classes to be descriptive and, in the case of fragments, you should name your fragment so that it's apparent what type of fragment you're subclassing. Once you create the new subclass, go ahead and add your tag and your factory method like you would any other fragment.

```
import android.app.ListFragment;
public class PresidentListFragment extends ListFragment {
	public static final String TAG = "PresidentListFragment.TAG";
	public static PresidentListFragment newInstance() {
		PresidentListFragment frag = new PresidentListFragment();
		return frag;
	}
}
```
