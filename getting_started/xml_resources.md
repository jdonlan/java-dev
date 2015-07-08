#XML Resources
Android defines a robust XML vocabulary that can be used to create application resources. In this section, we will talk about the advantages of using XML resources over those created in code. We will also cover some common types of XML resources and how each type is used.

##Advantages of XML Resources

The Android SDK defines a robust XML vocabulary that can be used to create application resources using XML instead of Java code. These resources range from a basic string resource, to settings values, to complete user interfaces. These resources are commonly defined using XML, but it should be known that every resource you define in XML can also be declared in Java code.

At this point you might be thinking, "If I can create all of my resources in Java, what's the point in using XML?" Well, there are several reasons for using XML but the main reason would be to keep your UI separate from the rest of your app. There's a software design pattern known as **MVC** (model-view-controller) that divides an application into three interconnected parts. By doing this, the app's data (model), the UI (view), or the mechanism by which your data and UI are altered (controller) can be easily swapped out without having to make major changes to the other two components.

For instance, say you've created an app that's been on the market for three months. In those three months, you've received a lot of feedback about how great the app works, but how ugly the UI is to look at. Using the MVC pattern and defining your app's UI in XML, you can easily go in and completely change the app's UI without ever having to change a single line of Java code. This greatly reduces the amount of work needed to make UI updates and also reduces the opportunity for bugs to be introduced since you're never touching the app's code.

Other advantages to using XML resources over those created in Java include:

* Reduces the amount of Java code in files that interact with the UI which reduces compile times.
* Altering a resource XML doesn't force your application to recompile which saves time when debugging layout issues.
* Resources are well organized into intuitively named folders.
* XML resources can be easily referenced through the R class which contains identifiers for every resource in the project's res folder.
* Layout XML files allow you to see your UI in the graphical editor instead of having to run the app to see your UI.
* Changes can be made in one XML file which updates every instance in your app where that resource is being used instead of having to update each individual code reference.

As mentioned earlier, the XML vocabulary in Android is incredibly robust and almost all of your resources can be defined in XML. The only exception to this would be for files like images, sounds, or any other specialized binary file that would be hard to represent with text. 

##Common XML Resource Types

###String
String resources are used to define text within an app. Of all of your different resource types, this is the on that it's encouraged you use XML the most. The reason for using an XML file to define all of your strings is the fact it makes localization very easy. Android provides a mechanism for providing the same set of string resources for several different languages at once if you declare your strings in XML. Having multiple language support can open your app up to new markets in non-English speaking countries.

###Layout
A layout XML defines the user interface for an application. The XML vocabulary contains keywords for different containers and user controls which can be created and positioned in an XML file as mentioned in the example above. Layout XML files in particular have an added advantage in that they can be created with the graphical layout editor that's built into the SDK. This allows you to drag and drop components into a UI and have the editor create the XML for you. Also, if you want to create your layout XML by hand, you could always check your changes in the graphical tool. This makes it so that you don't have to run your app just to see any UI changes you've made.

###Style
A style resource can define the look and feel of interface components in your application. If you want all of your buttons in your app to be blue to match your app's branding, you could do it in two ways. The first way would be to set each individual button to blue. Doing this leaves you open to mistakes as you might miss a button or you could accidentally change the color when making updates. The second would be to declare a button XML style that sets all buttons in the app to be blue. Using a style XML helps ensure that all elements are the same and that you only need to make a change in one place to change everything.

####Resources
http://developer.android.com/guide/topics/resources/available-resources.html
http://developer.android.com/guide/topics/resources/accessing-resources.html
