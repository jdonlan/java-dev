#ContentProvider
Content Providers are a core Android building block, much like Activity. Providers are primarily used for sharing data across multiple applications or providing search suggestions within a single application. Data access mechanism within a single application can also utilize Providers, but they're much more complex than using a database or file storage so there are few use cases for doing so.

Android includes many built-in Providers that can be utilized by other applications including...

* CalendarContract - Used to access calendar events and related information.
* ContactsContract - Used to access information about contacts stored on the user's device.
* MediaStore - Used to access information about the images, video, and audio files saved on the device.
* Telephony - Used to access MMS and SMS data.

While it seems counter-intuitive, ContentProviders are NOT a data source.  In fact, they are simply a mechanism that allows access to a data source through a uniform set of rules and a predefined structure.  Their purpose is to provide a layer of protection for data so that other applications can't directly access the data and cause possible harm.  In essence, they are the middle man between one application's data and other applications trying to use that data.

If ContentProviders aren't a data source, where is the actual data? The short answer is - anywhere.  ContentProviders can access data from anywhere within the Android system that it has access to.  While ContentProviders give the appearance and basic structure of a database, the data doesn't have to be actually stored in one.