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

The ListFragment class contains some special functionality in terms of creating a view for the fragment. With a list fragment, you don't need to create your own layout file and you don't need to override onCreateView(). Instead, the default implementation of onCreateView() will load a built-in resource that displays a list when you have data and a loading indicator while that data is loading. This makes it really easy to create a simple list, plus you get the added benefit of a loading indicator while you load data into the list. Instead of overriding onCreateView(), we'll ignore that method entirely and jump right to onActivityCreate().

In the onActivityCreated() method, we'll do all of our list setup. Use this method to load up your data and create a new adapter to hold that data. In our case, we'll load in an array of strings from our resource files and create a new ArrayAdapter to hold those strings. Once you have an adapter, you can set that adapter to the list in the fragment using the setListAdapter() method of the ListFragment class.

```
public class PresidentListFragment extends ListFragment {
	public static final String TAG = "PresidentListFragment.TAG";
	public static PresidentListFragment newInstance() {
		PresidentListFragment frag = new PresidentListFragment();
		return frag;
	}
	@Override
	public void onActivityCreated(Bundle _savedInstanceState) {
		super.onActivityCreated(_savedInstanceState);
		String[] presidents = getResources().getStringArray(R.array.presidents);
		ArrayAdapter<String> adapter = new ArrayAdapter<String>(getActivity(), android.R.layout.simple_list_item_1, presidents);
		setListAdapter(adapter);
	}
}
```

That's it! At this point you can add the list fragment to an activity the same way you would any other fragment and the list will load and display on the screen.

##Extra Functionality
In addition to just showing a list and interacting with it in an easy way, ListFragment also has some additional methods that are helpful in development. For example, setting an OnItemClickListener to a list is a very common task when working with ListView. When using ListFragment, you get a click listener built in. All you have to do is override the onListItemClick() method and you can handle click events. The parameters provided in onListItemClick() are the same parameters in an OnItemClickListener, they're just pre-casted to the proper types (e.g. AdapterView is casted to ListView).

```
public class PresidentListFragment extends ListFragment {
	public static final String TAG = "PresidentListFragment.TAG";
	public static PresidentListFragment newInstance() {
		PresidentListFragment frag = new PresidentListFragment();
		return frag;
	}
	@Override
	public void onActivityCreated(Bundle _savedInstanceState) {
		super.onActivityCreated(_savedInstanceState);
		String[] presidents = getResources().getStringArray(R.array.presidents);
		ArrayAdapter<String> adapter = new ArrayAdapter<String>(getActivity(), android.R.layout.simple_list_item_1, presidents);
		setListAdapter(adapter);
	}
	@Override
	public void onListItemClick(ListView _l, View _v, int _position, long _id) {
		String president = (String)_l.getItemAtPosition(_position);
		new AlertDialog.Builder(getActivity())
		.setTitle(R.string.president)
		.setMessage(getString(R.string.selected, president))
		.setPositiveButton(R.string.ok, null)
		.show();
	}
}
```

Another helpful method provided by the ListFragment class is the getListView() method. This method returns the ListView being used to show the list of data in the fragment. This is helpful in case you want to set the choice mode of the list or add on a long-click listener since that functionality isn't included in the base fragment. Also included in the ListFragment class is the setEmptyText() method which can be used to show the user some text if your list contains no data. If you were building a ListView layout yourself, you'd have to set this all up by hand. By using a ListFragment, you can easily show an empty list message with a single method call.

####References
http://developer.android.com/reference/android/app/ListFragment.html