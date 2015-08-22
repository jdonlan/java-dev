#Android Resources
In Android, Resources are accessible content stored within the application. Many forms of content can be stored here, from strings and numbers, to images and colors, and even layouts and animations. The Android platform utilizes a single common resources directory /res for developers to store content which will be accessed throughout the application. Proper utilization of the resources directory allows the platform to assist with a variety of advanced functions such as content localization, graphics display at various resolutions, and even supporting orientation changes during use.

##Structure
As the resources directory holds many types of content, knowing and utilizing the appropriate structure and conventions will allow a developer to take full advantage of the platform. When using Android Studio, creating a new Android project will automatically create a resource folder with a few sub-directories already in place. The most common resource directories are as follows:
* ***/drawable-density*** - store image resources for the associated screen pixel density.
* ***layout*** - store Android XML layout files to establish the views and UI of your application.
* ***menu*** - build system menus for advanced controls in your application views.
* ***value*** - store data values for content in the default language for the application as well as style and design data.

##Accessing Resources
When an Android application compiles, the system generates a convenient single class pointer which contains resource content within the application - R. This system constant provides developers with a consistent reference to any resource needed throughout the development process. Resources can be accessed by referencing the R class, then narrowing to the specific resource needed. The call to a resource is broken into three parts - R.type.resource_id. Looking at the resource reference, R is the system constant, while the resource ID is a defined unique identifier assigned by the developer to a given resource. The only remaining part of the reference is the type. The type is a call out to one of a few pre-defined Android resource types which are typically in line with their location in the /res folder and/or the resource function. The types are documents in the referenced Android Developer Guide with detailed information on each type, the associated /res directory, and the content.

###Examples
Accessing a String resource with the id of "application_name"
```
R.string.application_name
```
Accessing a layout with the id of main_activity
```
R.layout.main_activity
```

##The Power of Resources
The real advantage of utilizing Android Resources isn't just a convenient central way of storing and accessing content, but actually the extensibility it offers to application development. The Android resource system allows developers to easily add functions and features that add significant value to the application overall.

###Localization
As proper Android design dictates utilizing the String resource structure to hold string content centrally, replacing strings to support additional languages can actually be handled utilizing the resource structure. To add support for French, for example, a developer simply has to provide the application with a French string directory as part of their resource structure with the translated strings in place.

```
res/
   values/
       strings.xml
   values-fr/
       strings.xml
```

As Android utilizes the R.type.resource_id structure to access resources, the platform itself will adjust the directory which it looks for resource values based on the application's locale preference. As such, the French strings.xml resource file simply needs to utilize the same ids for the translated strings, and the application will translate automatically.

```
Mon Application
Bonjour le monde !
```

###State Changes & Orientation
The Android resource system can also be used to handle application state changes and orientation changes. This is handled similarly to how locale changes are handled for languages. For example, a layout which is built for a portrait orientation stored in /res/layout can have an alternate layout, optimized for a landscape orientation stored in the /res/layout-land directory.

Additionally, the platform can go one step beyond simply handling orientation changes and also support customized layouts for the wide variety of Android devices on the market. As with localization and orientation changes, this can be done with a simple addition of a resource file in the appropriate structure. To support screens on a typical 7" tablet, for example, all that is required is an optimized layout resource file stored in */res/layout-sw600dp* or */res/layout-sw600dp-land* for portrait and landscape respectively. The sw600dp represents the approximate 600 device pixels on a 7 inch screen.

####References
[http://developer.android.com/guide/topics/resources/index.html](http://developer.android.com/guide/topics/resources/index.html)
[http://developer.android.com/training/basics/supporting-devices/languages.html](http://developer.android.com/training/basics/supporting-devices/languages.html)
[http://developer.android.com/training/basics/supporting-devices/screens.html](http://developer.android.com/training/basics/supporting-devices/screens.html)

