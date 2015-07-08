#Fragment Usage

##Encapsulating Features
Let's say we're building a calculator app. In this calculator we'll have two modes, simple and scientific, that the user can select via a menu item. Without fragments, we'd have to put all of the simple logic in the activity alongside the scientific logic in a single activity, or have multiple activities with duplicate functionality, to support each mode. We'd have to swap out the UI or entire activity depending on which type of calculator the user selects. 

Looking at the app another way, the simple and scientific modes are actually two separate features of the calculator app. As such, we create utilize a fragment for the simple mode and another fragment for the scientific mode, each providing their added features the base calculator. When the user selects simple, we show the simple fragment. When the user selects scientific, we show the scientific fragment. This keeps the simple logic from interfering with the scientific logic and, since each fragment handles its own UI, we don't have to do any extra logic to check which calculator UI is showing.

##Supporting Multiple Layouts
In addition to making a single activity more modular as described above, fragments also make it easy to support multiple screen sizes. The best example of how this works would be a master/detail application.

Building a master/detail app using only activities would generally require three activites. On a phone sized device, you would have a master activity to show your list and a detail activity to show your details view. Then for tablets, you'd have one master/detail activity that showed both views side by side. This could possibly be cut down to two activities with the master activity serving double duty in phone and tablet layouts, but that would cause your master activity to become very bloated with unnecessary code in phone mode.

In building your application with multiple activities like this, you're duplicating a lot of code. The master and detail activity code is almost entirely duplicated into the single master/detail activity used for tablets. This means that you'd have to maintain three separate activities and update all code in two places in order to make any updates to the master/detail functionality. To make this a bit simpler, we can use fragments.

In the fragment version of this app, we'll have two activities and two fragments. This might sound like more code right off the bat, but it's actually less in the end. For our two fragments, we'll have a master fragment and a detail fragment. In our master activity, we'll have a single pane layout for phones and a two pane layout for tablets. For our detail activity, we'll just have a single pane layout for phones and we won't even use that activity for tablets. For phones, we'll put the master fragment in the single pane master activity. Then when the user clicks on an item, they're taken to the details activity that holds the details fragment. In tablet mode, we'll put the master fragment in the first pane of the layout and the details fragment in the second pane. Then, when the user clicks on an item in the master fragment, we'll just show the results in the details fragment right next to it. An example of what this looks like can be seen below. Activity/fragment A would be the master and activity/fragment B would be the detail.

![](master_detail.png)

In the fragment example, we're now only maintaining a single master fragment and a single details fragment. Whenever we need to make updates, we only need to make them in one place. For this example, we don't need to update the activities at all because they're only containers that are used to hold the fragments. Not only is this less code to maintain, but it reduces the risk of bug introduction since each fragment acts as a module and can be updated independent of the other.

####References
http://developer.android.com/guide/components/fragments.html