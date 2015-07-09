#ContentProvider
Content Providers are a core Android building block, much like Activity. Providers are primarily used for sharing data across multiple applications or providing search suggestions within a single application. Data access mechanism within a single application can also utilize Providers, but they're much more complex than using a database or file storage so there are few use cases for doing so.

Android includes many built-in Providers that can be utilized by other applications including...

* CalendarContract - Used to access calendar events and related information.
* ContactsContract - Used to access information about contacts stored on the user's device.
* MediaStore - Used to access information about the images, video, and audio files saved on the device.
* Telephony - Used to access MMS and SMS data.

While it seems counter-intuitive, ContentProviders are NOT a data source.  In fact, they are simply a mechanism that allows access to a data source through a uniform set of rules and a predefined structure.  Their purpose is to provide a layer of protection for data so that other applications can't directly access the data and cause possible harm.  In essence, they are the middle man between one application's data and other applications trying to use that data.

If ContentProviders aren't a data source, where is the actual data? The short answer is - anywhere.  ContentProviders can access data from anywhere within the Android system that it has access to.  While ContentProviders give the appearance and basic structure of a database, the data doesn't have to be actually stored in one.  That said, as the structure of ContentProviders mimic databases, it does make it easy when the data is actually coming from a database.  Built-in providers, such as those listed above, use SQLite databases as their data source.

##Working with Provider Data
ContentProvider accessible data is accessed using the ContentResolver class. You can retrieve a ContentResolver instance by calling the getContentResolver() method of the Context class. ContentResolvers allow you to perform the following functions to a ContentProvider: insert, update, delete, and query. Sound familiar?

All ContentResolver methods that access a provider work very similarly to how a SQLiteDatabase works. The one key difference is that you don't specify a table name for your actions, you specify a content URI that points to a specific ContentProvider. Every provider has a unique URI that allows you access to its data.  That URI changes slightly based on the data that you want to access.

###Dissecting the URI

```
content://user_contacts/base_data/1
```

**content://** - The schema for the URI that signifies that we're accessing a ContentProvider.

**user_contacts** - The authority or domain that tells the system which provider to access.

**base_data** - The "table" or data source in the provider we want to access.

**1** - The "row" or item in the data source or table that we want to access. This piece of data is entirely optional and should only be used if you know exactly which data point you want to access.

The URI for a provider can be a long and complicated name which opens you up to annoying errors caused by typos and misspellings. Additionally, keeping track of all available table and column names can be quite a pain.
Contracts, as you may recognize from the built-in providers, can solve this issue.

###Contracts
A contract is a type of class that goes along with a ContentProvider to provide uniform access to commonly used values. Contract classes are just like any other class except that they should be marked as final and only contain variables and inner classes that are marked as static and final in their declaration. They are not required to use content providers, but they make life much easier so you should create one to go with your provider whenever possible.

####Example Contract

```
public final class DataContract {

    // The URI that points to our provider on the device.
    public static final String CONTENT_URI = 
        "content://com.example.provider/";

    // The name of our data source inside the provider.
    public static final String DATA_SOURCE = "data_table";

    // For easy access, we'll provide a single constant for the combined URI.
    public static final String DATA_SOURCE_URI = 
        CONTENT_URI + DATA_SOURCE;

    // The column names for accessing data in our provider.
    public static final String ID = "_id";
    public static final String FIRST_NAME = "_first_name";
    public static final String LAST_NAME = "_last_name";
    public static final String AGE = "_age";
}
```

As you can see in the above contract, the use of static final strings alleviates potential problems with using strings for data components.  As the IDE can recognize these constants, we can let it do the heavy lifting with forming our URI.

###ContentResolver Operations

####insert
The insert method allows you to insert data into a ContentProvider. This method should return a Uri object that points to the newly added item.

```
// First we need to turn our URI string into a Uri object.
Uri accessUri = Uri.parse(DataContract.DATA_SOURCE_URI);

// Form a ContentValues object of the data we want to insert.
ContentValues values = new ContentValues();
values.put(DataContract.FIRST_NAME, "John");
values.put(DataContract.LAST_NAME, "Smith");
values.put(DataContract.AGE, 237);

// Insert data into the database and optionally catch the return.
Uri newItem = getContentResolver().insert(accessUri, values);
```

####update
The update method allows you to update data in a ContentProvider based on a selection. If you don't provide a selection, all data will be updated. Method returns the number of rows that were updated.

```
Uri accessUri = Uri.parse(DataContract.DATA_SOURCE_URI);

// Form a ContentValues object of the updated data.
ContentValues values = new ContentValues();
values.put(DataContract.FIRST_NAME, "Gary");
values.put(DataContract.AGE, 26);

// Form our selection statement so that we don't update all data.
String whereClause = DataContract.ID + "=?";
String[] whereArgs = new String[] { "1" }; // Assume we're targeting row 1.

// Update our provider with the new data.
int updatedRows = getContentResolver()
    .update(accessUri, values, whereClause, whereArgs);
```

####delete
The delete method allows you to delete data in a ContentProvider based on a selection. If you don't provide a selection, all data will be deleted. Method returns the number of rows that were deleted.

```
Uri accessUri = Uri.parse(DataContract.DATA_SOURCE_URI);

// Form a selection statement so that we don't delete all.
String whereClause = DataContract.ID + "=?";
// Assume we're targeting row 1 for deletion.
String[] whereArgs = new String[] { "1" };

// Delete the targeted data from our provider.
int deletedRows = getContentResolver()
    .delete(accessUri, whereClause, whereArgs);
```

####query
The query method allows you to query for any number of rows or columns in a ContentProvider. You can specify specific columns or just get all of them. Optionally you can specify a sort order. This method returns a Cursor that contains the returned data.

```
Uri accessUri = Uri.parse(DataContract.DATA_SOURCE_URI);

// We'll only query for the id, first name, and last name.
String[] columns = new String[] {
    DataContract.ID,
    DataContract.FIRST_NAME,
    DataContract.LAST_NAME
};

// Query for all entries and get their id, first name, and last name.
Cursor nameDataCursor = getContentResolver()
    .query(accessUri, columns, null, null, null);
```

##Building a ContentProvider
Start with a class that extends ContentProvider and overrides the required methods.
All providers must be registered in the manifest using the &lt;provider&gt; tag.
All providers must provide a URI authority, more commonly referred to as a domain when it comes to web URIs. 
```
content://com.company.android.provider/table_name/
```

The authority for your provider defines the URI that is used to access your ContentProvider from other applications. All providers are accessed via a URI that uses the content schema (content://). The authority for your provider should be unique across all applications. Because of this, your package name makes a good authority name. Content providers can have multiple authorities for pointing to different data sources, but we'll only be covering single source providers in this book.

In addition to a unique authority name, your provider can also define permissions to help safeguard against unauthorized access. Permissions can be defined in the manifest using the android:permission property of the provider tag and the &lt;permission&gt; tag. Permission names should be unique so that other apps don't accidentally gain access to your data and should start with the authority name of your provider. If the permission name doesn't start with the content provider authority, they will not work.

###Provider Tag Example
```
<!--
    Add your permission to the manifest so other apps can access it.
    This goes right underneath your uses-permission tags. 
 -->
<permission android:name="com.company.android.provider.ACCESS_DATA"
    android:protectionLevel="dangerous" />

<!-- 
    Providers should be enabled and exported so
    that you can access them from other apps.
 -->
<provider android:name=".MyProvider"
    android:authorities="com.company.android.provider"
    android:enabled="true"
    android:exported="true"
    android:permission="com.company.android.provider.ACCESS_DATA" >

</provider>

<!-- 
    Any app that uses your provider must add the 
    defined permission to their manifest like so: 
 -->
<uses-permission android:name="com.company.android.provider.ACCESS_DATA" />
```

