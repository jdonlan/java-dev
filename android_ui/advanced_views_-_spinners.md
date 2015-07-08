#Advanced Views - Spinner

Spinners are Android's take on the classic drop down menu. They provide users with the ability to select one or more options from a list of items with minimal screen real estate required. Alongside Lists and Grids, Spinners are a UI option for presenting the user with the ability to interact with a collection of information. The Spinner is typically the control of choice when presenting several options to the user alongside other visual elements on the screen.

###Benefits
As Spinners have, for most intents and purposes, shared functionality with Lists and Grids, it is important to understand when you should use a Spinner over the alternatives. The answer to this question often is found in the desired user experience. As Grids and Lists tend to take up generous amounts of screen real estate, Spinners are often the best choice when you don't have that real estate to spare. 

When creating a form that accepts multiple pieces of information, for example, a Spinner allows the application to present choices for certain information items, while allowing room for other UI controls for user input. Take the following image from the Android Developer site:

![](spinner.png)

In this image, the application is collecting an email address, and presenting the user with choices for the nature of that email address in a Spinner. A List or Grid would not provide the best user experience in this case, as it would not be evident of the relationship between the email text entry and the email type options being presented.

Another prime use of the Spinner is in conjunction with the Android ActionBar. As the ActionBar exists as a single horizontal section at the top of an application, a Spinner is a good choice for providing the user with selections in that limited space such as navigation menus.

###Drawbacks
The primary drawback of using a Spinner within a UI is the fact that it takes a minimum of 2 user interactions to make any meaningful selection. First, the user must touch the Spinner to activate the control and display the options, then the user must select an item from the displayed options, or touch outside of the Spinner to cancel the interaction.  Further, if the desired option is not initially visible to the user, the user will have to scroll to find the desired selection, creating a 3rd required interaction.

##Populating Spinners
Spinners, like other collection Views, are fed through Adapters. These adapters attach data collections to the individual views presented to the user when the Spinner is activated. This can be done using either a static source of data, such as Android Resources, or dynamic data fed to the application at runtime. In either approach, the Adapter must be created and populated, then attached to the Spinner in order to load the information into the control. Refer to the Adapters sections for more information on how to create and populate Adapters.

##Events and Handlers
*OnItemSelectedListener* reacts to users touching an item in an activated Spinner. The Listener checks for two primary callbacks

###onItemSelected
This callback fires when a user touches an item in a Spinner. The callback is passed the parent of the item selected, the view for the item that was selected, the position of the selected item in the adapter, and unique identifier for the item.

###onNothingSelected
This callback fires when a selected item in the Spinner is removed from the view.

####References
http://developer.android.com/guide/topics/ui/controls/spinner.html