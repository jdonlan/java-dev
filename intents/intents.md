#Intents
One of the most versatile benefits of the Android platform is the ability for the system to pass data between not only screens or views within an application, but across applications entirely. This is accomplished through the power of Android Intents.

To understand Intents, you must first understand Activities. As applications are typically a series of activities, each with their own respective functionality and purpose, there needs to be a mechanism which allows them to communication with each one another. Intents communicate with the Android OS to tell the system the activity is requesting functionality from an external source. Think of the Intent as a tunnel to pass parameters from activity to activity. There are two types of intents:

**Explicit Intents** - An explicit intent informs the system which external class is capable of handling the functionality request. This is generally a class within the application.
**Implicit Intents** - An implicit intent is a request to the system for functionality. The system then looks for activities registered with the system that provide that functionality and presents the user with options on which activity they would like to use.

##Using Intents
To create an Intent you will call the constructor for the Intent Class. There are various constructors of the Intent class depending on the desired handling for the Intent. Once defined, an intent is used to start an activity through one of a few methods. Some activities are started blindly with no expectation of returning to the previous activity. For example, a PDF document might include a link to a website. Once clicked, the PDF reader activity would start an activity that would load the URI into another activity capable of displaying websites.

Other activities may expect data to be returned. For example, a contact manager might allow the user to attach a photo from the camera. As such, the contact management activity would need to launch an activity which captures image data from the camera. This activity would then pass that data back to the contact activity so that it could be attached to the contact record.

And yet other activities might need custom data functionality from a separate component from within the same application. For example, an application might have a primary activity, but also support user preferences. The user preferences would then be updated in a separate activity and saved, then the application focus would need to revert to the primary activity.

Intents are the vehicle that request operations from the system and carry the data back and forth between activities at launch and completion in each of these situations. They carry the data through a Bundle object using a key value system similar to objects.