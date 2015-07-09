#Shared Preferences
Saving data out to a file is a good solution for saving large amounts of, usually complex, data. However, what if you only need to save a few simple values? When building a settings page for an app, for example, you typically need to store simple values such as a boolean to say whether a feature should be turned on or off. Instead of creating a new file and keeping track of the structure of that file to store these values, Android provides a key/value storage mechanism known as shared preferences.

Shared preferences allows you to store key/value pairs which can be used to save these small bits of data used in an application. Preferences aren't used to store complex objects and they're also not intended to store lists of data. They're only used to store simple data points and primitive types. Behind the scenes Android stores this data in a hidden XML file. This file cannot be accessed directly, but is instead accessed through the SharedPreferences class.

The SharedPreferences class provides easy access to preference data and a secure editor for editing preferences. To use the shared preferences, you can either use the default preferences or create your own. The default preferences for any app can be accessed using the PreferenceManager class and should be used to store settings data that has an effect on the app as a whole. If you need to store preferences for a specific feature in an app, it would be more appropriate to make your own preferences. An example of each is shown below:

```
// "this" refers to a Context
SharedPreferences defaultPrefs = PreferenceManager.getDefaultSharedPreferences(this);
 
private static final String PREFS = "package_name.PREFS";
SharedPreferences custom = this.getSharedPreferences(PREFS, Context.MODE_PRIVATE);
```

You'll notice that the naming for our preference file in the custom example starts with the package name. It's possible to make a preference file visible to other applications and, if you do, it's best to use your app's package name to identify the file. This way your preference file isn't confused with a preference file from another application.

Once you have an instance of SharedPreferences, getting and setting preference data is quite simple. To get data, you just use one of the several get methods described in the SharedPreferences class documentation. Get methods exist for several, but not all, basic types as seen below. Getting data from shared preferences works exactly like getting data from a HashMap, assuming the key for the map is always a string. The first parameter of the get method is the key that points to the desired value and the second parameter is a default value to return in case the preference file doesn't contain the requested key. You can specify any default value you want whereas a map's default value is null.

```
SharedPreferences defaultPrefs = PreferenceManager.getDefaultSharedPreferences(this);
 
String data = defaultPrefs.getString("data", "");
 
int integerData = defaultPrefs.getInt("integerData", 0);
 
boolean boolData = defaultPrefs.getBoolean("boolData", false);
```

While getting data is a simple method call, setting data requires a couple extra steps. To edit preference data, we must use the SharedPreferences.Editor interface. After obtaining an instance of SharedPreferences, we can call the edit() method to get an Editor instance that works specifically for that set of preferences. Each Editor only works for the preferences which created it. Once we have an editor, we can start adding data to our preferences with the put methods of the Editor interface. For every get method in SharedPreferences, there is a put method in the Editor. Lastly, once all changes are made, you must apply those changes using the apply() or commit() methods of the Editor interface. Either method will work but commit() will return a boolean to say whether or not the changes were successfully applied.

```
SharedPreferences defaultPrefs = PreferenceManager.getDefaultSharedPreferences(this);
 
// Get a new editor
SharedPreferences.Editor edit = defaultPrefs.edit();
 
// Set some preference data
edit.putString("data", "Some Data");
edit.putInt("integerData", 100);
edit.putBoolean("boolData", true);
 
// Apply all changes
edit.apply();
```

####References
http://developer.android.com/reference/android/preference/PreferenceManager.html
http://developer.android.com/reference/android/content/SharedPreferences.html
http://developer.android.com/reference/android/content/SharedPreferences.Editor.html


