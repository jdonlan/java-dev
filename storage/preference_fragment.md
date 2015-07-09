#PreferenceFragment
If you look through several Android apps and even the system settings app on an Android device, you'll notice that settings pages have a very well defined look and feel to them. This isn't a coincidence as the Android SDK contains a type of fragment, PreferenceFragment, that is used to build these settings pages. When building a settings page for your own app, you too should use this fragment.

The look and feel provided by the PreferenceFragment should be used across all settings pages in almost all apps. This way the user knows exactly how to change settings and knows exactly what to expect when interacting with different preferences on the settings page. Providing a user experience that is consistent with other Android settings pages will allow your user to more comfortably use that portion of your application.

In addition to providing a consistent user experience for interacting with settings pages, the PreferenceFragment also provides an easy way to interact with SharedPreferences files. That's because that every setting changed in the settings UI, also changes an underlying preference file. Using a PreferenceFragment cuts down on the UI work you need to do in creating this page of your app, and it also cuts down on the backend code you need to write to store out the user settings. Now that you know why we use preference fragments, let's take a look at how they actually work, and why this section of the book is located here rather than the chapter on Fragments.

##Using a PreferenceFragment
The UI for a PreferenceFragment has a defined look to it. Unlike other fragments or layouts, however, you don't build this UI with views in a layout. Instead, we build a preference XML file. We'll start off by creating a new folder in our "res" directory named "xml" and we'll add a new "settings.xml" file to it. Inside that XML file, we'll define a root tag named PreferenceScreen.

```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android" >
 
</PreferenceScreen>
```

Instead of using actual layouts, the PreferenceScreen tag is used to define our preference "layout". Now that we have our "layout" root, we can define the actual preferences that we want to show to the user. Much like layouts have several views that you can add, preference screens have a few different preference types you can add to them.

* Preference: A generic preference that accepts click events.
* ListPreference: A preference that allows the user to select from a list of pre-defined values.
* EditTextPreference: A preference that accepts text input.
* CheckBoxPreference: A preference that accepts boolean (checked/not-checked) values.

There are more preference types than what's listed above, but these are just a few of the common types. Just like with layouts and views, these preferences have corresponding XML tags which can be used to place them inside of a PreferenceScreen. The main difference between preferences and views though is that all preferences are arranged in a vertical stack in the order they appear in the XML. Preferences don't have layout parameters like you would have when working with views, so you can't rearrange them. Let's add a few preferences to our screen and take a look at the required parameters for defining each.

```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android" >
 
</PreferenceScreen>
```

When defining a simple clickable preference, there are only two required parameters you must add, key and title. The key parameter is used to define the key to be used when accessing the value of this preference in shared preferences. So if every preference is a key/value pair, this key parameter defines the key for the preference pair. The title is used to define the first line of text shown on the preference. The title should be a short string that describes the preference. The summary is the subtext shown on a preference. The summary should be a short (but longer than the title) string that describes what this preference does. Summary fields are completely optional but should always be included if the title isn't sufficient for describing the preference.

When defining a ListPreference, there are a few extra fields that we need to fill out. The first is the entries field. This field takes in a string array, typically defined in the resources, that represents all of the entries in the list that are shown to the user. So when the user views the list, this is the data they'll see. The other array you see, entryValues, describes the actual values of each entry. For instance, if your list preference was to select the number of employees on a team, the list might show "Two Employees" while the actual value of "Two Employees" could just be 2. Then the last parameter, defaultValue, defines what the default selection in the list should be.

In addition to defining preferences for each preference screen, you can also define preference categories which can be used to group related preferences into named groups. To do this, simply wrap your preferences in a PreferenceCategory tag and give it a title.

```
<?xml version="1.0" encoding="utf-8"?>
<PreferenceScreen xmlns:android="http://schemas.android.com/apk/res/android" >
 
    <PreferenceCategory android:title="Demo Preferences" >
 
        <Preference
            android:key="PREF_CLICK"
            android:summary="A clickable preference item."
            android:title="Click Preference" />
 
        <ListPreference
            android:key="PREF_LIST"
            android:summary="A preference that shows a list of values"
            android:title="List Preference"
            android:entries="@array/entries"
            android:entryValues="@array/values"
            android:defaultValue="Item 1" />
 
    </PreferenceCategory>
 
</PreferenceScreen>
```

Now that we have our preference screen defined in XML, it's time to hook it up in code. To do this, we start off by defining a new subclass of PreferenceFragment.

```
public class SettingsFragment extends PreferenceFragment {
 
}
```

The next thing we need to do is load in all of the preferences for our preference screen from the "setting.xml" file that we defined earlier. To do this, we override the onCreate() method of the fragment and load the XML file using addPreferencesFromResource(). This method will parse out the PreferenceScreen XML file and generate a UI that shows all of the preferences listed in our XML. At this point, the screen should look correct and work just as well.

```
public class SettingsFragment extends PreferenceFragment {
 
	@Override
	public void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
	
		// Load the preferences from our XML file.
		addPreferencesFromResource(R.xml.settings);
	}
}
```

The only thing left for us to do on this screen is to make that generic preference functional. The ListPreference works on its own since it has a list of values from which the user can choose. However, the generic preference we defined doesn't do anything on it's own. To make this do something, we can add an OnPreferenceClickListener to it. First, we'll need to find our preference using the findPreference() method of the PreferenceFragment class. That method will return to us a Preference object to which we can attach the listener. From there, you can do whatever you wish.

```
public class SettingsFragment extends PreferenceFragment {
 
	@Override
	public void onCreate(Bundle _savedInstanceState) {
		super.onCreate(_savedInstanceState);
	
		addPreferencesFromResource(R.xml.settings);
	}
	
	@Override
	public void onActivityCreated(Bundle _savedInstanceState) {
		super.onActivityCreated(_savedInstanceState);
		// Find preference by key
		Preference pref = findPreference("PREF_CLICK");
		pref.setOnPreferenceClickListener(new OnPreferenceClickListener() {
			@Override
			public boolean onPreferenceClick(Preference _pref) {
				// Do what you want
				return true;
			}
		});
	}
}
```

All you have to do now is add this fragment to an activity, and it's ready to be used. If you ever want to retrieve the preference values set by a PreferenceFragment, you can do so using the default preferences returned by the PreferenceManager.

####References
http://developer.android.com/reference/android/preference/PreferenceFragment.html

