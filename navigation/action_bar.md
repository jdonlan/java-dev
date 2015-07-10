#Action Bar
The Action Bar is a window feature that resides at the top of an Activity and considered one of the most important design elements for your app providing numerous advantages:

* Makes important actions prominent and accessible in a predictable way (such as New or Search).
* Supports consistent navigation and view switching within apps.
* Reduces clutter by providing an action overflow for rarely used actions.
* A dedicated space for giving your app an identity.

##Implementing the ActionBar
For apps supporting API 11 and higher, the following class should be added to the imports:

```
import android.app.ActionBar
Next you will ensure that the Action Bar method is implemented:

@Override
  public boolean onCreateOptionsMenu(Menu menu) {
      MenuInflater inflater = getMenuInflater();
      inflater.inflate(R.menu.main, menu);
      return true;
  }
```


##Elements of the Action Bar
* The App Icon
* Action Items
* Overflow Items

###The App Icon

This is the image at the top of the action bar that represents the app and can be enabled to act as the 'Up" navigation.

###Action Items
The action bar allows the user to navigate through the use of the action items. These items appear directly in the action bar with an icon or text known as the action buttons. Those actions that cannot fit in the action bar or are considered lower priority show up in the action overflow. This is accessible by the button on the far right of the action bar that has 3 dots in a vertical line.

To define the action items, you must implement an xml defintion in the menu directory in the resources.

```
<menu xmlns:android="http://schemas.android.com/apk/res/android" >
    <item android:id="@+id/action_settings"
          android:icon="@drawable/ic_action_settings"
          android:title="@string/action_settings"
          android:showAsAction="always"/>
    <item android:id="@+id/action_search"
          android:icon="@drawable/ic_action_search"
          android:title="@string/action_search"
          android:showAsAction="ifroom"/>
</menu>
```

###Overflow Items
The action overflow int he action bar provides access to the app's less frequently used actions.

In the XML code above, the settings icon is set to always show in the action items. The search icon is set to show if there is room in the action items, and there's isn't room, to display in the overflow items.

###Handling an action bar selection
```
@Override
  public boolean onOptionsItemSelected(MenuItem item) {
    switch (item.getItemId()) {    
        case R.id.action_settings:
          // Implement code to execute for the settings
          break;
        case R.id.action_search:
          // Implement code to execute for the search
            openSearch();
        default:
          // Implement default behavior here
          break;
    }
    return true;
  }
```

###Split Action Bar
Split action bar provides a separate bar at the bottom of the screen to display items so that the navigation items and the title can be displayed in a reasonable amount of space.

See the XML definition below used for split action (API 14 and higher)

```
<activity uiOptions="splitActionBarWhenNarrow">
    <meta-data android:name="android.support.UI_OPTIONS"
               android:value="splitActionBarWhenNarrow" />
</activity>
```

###Enabling the App Icon for Up navigation
The Up navigation allows the user to navigate between screens. For example with a Master/Detail design, the app would have a Master view with a list of items and a Detail view with data related to the list item selected on the Master view. After the user selects the list item and is viewing the Details, the Up navigation brings the user back to the list on the Master view.

To enable the app icon as an Up button, call setDisplayHomeAsUpEnabled().

See the code below used to enable the Up navigation.

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_details);

    ActionBar actionBar = getActionBar();
    actionBar.setDisplayHomeAsUpEnabled(true);
}
```
