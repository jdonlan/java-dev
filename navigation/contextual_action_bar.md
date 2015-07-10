#Contextual Action Bar
While the ActionBar provides utility at the Activity level, users will often need to interact with individual elements within the UI of a given application. The Android Contextual Action Bar enables an application to provide enhanced interaction with specific elements within an application. A gallery application, for example, might allow a user to select multiple pictures to share online with family and friends, without the need to share each picture individually. In this example, a "share" control would not be a valid behavior without the context of one or more selected images. As the gallery would load without any images selected, the Action Bar should not display this control by default. This would be an ideal situation to implement a Contextual Action Bar. Once a user selects an image, the application would load a Contextual Action Bar that would then display the share icon, allowing the user to share the image.

##Action Mode
Contextual Actual Bars (CABs) require an implementation of the ActionMode.Callback interface. This interface is contained within an instance of the ActionMode class and requires 4 methods to be defined. These methods allow the Callback instance to handle user interaction events with the Contextual Action Bar. These methods are as follows:

* onCreateActionMode(ActionMode, Menu) once on initial creation
* onPrepareActionMode(ActionMode, Menu) after creation and any time the ActionMode is invalidated
* onActionItemClicked(ActionMode, MenuItem) any time a contextual action button is clicked
* onDestroyActionMode(ActionMode) when the action mode is closed
* onCreateActionMode

This method is similar to onCreate for an Activity in that it is fired when the CAB is started. It inflates the initial menu and displays the CAB in the application.

###onPrepareActionMode

This method is used when dynamic updates need to be made to the CAB. Using the same gallery application example, the user may select an initial image, providing a CAB with a set of options. If the user were then to select more images, the menu may need to be altered to reflect the options the user has for multiple images. A single image, for example, may show an edit option, whereas multiple images may not support a mass edit function.

###onActionItemClicked

This method behaves similar to a stand menu item selection in Android. The method will typically determine which item was interacted with an trigger the appropriate behavior within the application.

###onDestroyActionMode

This method is called under two conditions:

The user clears the CAB by selecting the check mark created by default within the CAB, indicating the user does not want to perform a contextual action.
The CAB is finished programmatically by calling the mode.finish() method after a user interaction.
mode.finish() is typically called after a user interaction where the CAB no longer applies. Sticking with the gallery example, if a user selects an image to display the CAB, then selects the delete menu option from the CAB, the application no longer has a selected image context, and thus the CAB no longer applies.

##Using the Contextual Action Bar

The actual implementation of a Contextual Action Bar is relatively straight forward. In fact, the most basic implementation is only a single line in an Activity:

```
startActionMode(<ActionMode.Callback>);
```

As is typical, however, best practice suggests a few more steps that ensure consistent behavior for the user.

###Storing a reference to the ActionMode

It is typically a good idea to store a reference to the ActionMode to avoid opening multiple Contextual Action Bars. This is done when starting the ActionMode, as a reference is automatically returned by the startActionmode method.

```
private ActionMode mActionMode;
mActionMode = startActionMode(<ActionMode.Callback>);
```

Now that a reference is stored for the ActionMode, the application should prevent new CABs from being launched without the user explicitly clearing the current CAB. To do this, we simply add some logic to our CAB launching event to check for an existing ActionMode before starting a new one.

```
if(mActionMode != null){
  return false;
}
mActionMode = startActionMode(<ActionMode.Callback>);
return true;
```

###Clearing the reference to the ActionMode

As we are preventing the application from starting multiple ActionModes, we need to ensure the application releases the reference when the user interacts with the CAB. As mentioned in the Callback method descriptions above, this is done in the onDestroyActionMode() method of the Callback.

```
mActionMode = null
```

###Responding to User Interaction

The core functionality of the CAB is executed in the onActionItemClicked() method. It functions similar to standard menu interactions.

```
public boolean onActionItemClicked(ActionMode mode, MenuItem item) {
  switch (item.getItemId()) {
    case R.id.myMenuItem:
      //MENU CODE
      return true;
    default:
      return false;
  }
}
```

####References
http://developer.android.com/guide/topics/ui/menus.html#CAB
http://developer.android.com/reference/android/view/ActionMode.html