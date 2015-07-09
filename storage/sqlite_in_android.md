#SQLite in Android

##SQLiteOpenHelper
The SQLiteOpenHelper is a class that handles creating and saving of a database to private storage. It provides simple callbacks to override for creating and upgrading a database and contains methods for easily accessing a read/write enabled database object.

```
public class DatabaseHelper extends SQLiteOpenHelper {

	private static final String DATABASE_FILE = “database.db”;
	private static final int DATABASE_VERSION = 1;

	private static final String CREATE_TABLE = “CREATE TABLE IF NOT EXISTS example (“ +
		“_id INTEGER PRIMARY KEY AUTOINCREMENT, some_text TEXT)”;

	public DatabaseHelper(Context c) {
		super(c, DATABASE_FILE, null, DATABASE_VERSION);
	}

	@Override
	public void onCreate(SQLiteDatabase db) {
		db.execSQL(CREATE_TABLE);
	}

	@Override
	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
		// Handle upgrade as necessary.
	}
}
```

##SQLiteDatabase
The SQLiteDatabase class that represents an actual sqlite database loaded into memory. It can use the execSQL method to execute SQL statements that perform actions or rawQuery to execute SQL query statements and retrieve a result.  It also contains helper methods for working with the database such as insert, delete, query, and update.

###ContentValues
ContentValues are used to pass in key/value pairs for column names and values into a database functioning similarly to a Bundle or HashMap. Multiple "put" commands are used to build the values.

###insert()
This method takes in three parameters: table name, a null column hack, and a set of values. The null column hack is a string that represents a column name to insert if no values are passed in. This is needed if your values can be null, otherwise, you can set this parameter to be null. The values parameter is a ContentValues object that contains the key/value pairs representing column names and values.

```
ContentValues v = new ContentValues();
v.put(“title”, “Example Title”);
v.put(“description”, “Example Description”);
v.put(“publish_date”, “2014-10-19 21:06”);
v.put(“url_string”, “http://example.com”);

// ID of the newly inserted row.
long id = db.insert(“articles”, null, v);
```

###update()
This method takes in four parameters: table name, updated values, a WHERE clause, and WHERE arguments. The updated values parameter is a ContentValues object containing the updated column values.The WHERE clause parameter is a WHERE clause without the “WHERE” keyword. The WHERE arguments are any values you want to insert into the WHERE clause using a “?” wildcard in the WHERE text.

```
String[] whereArgs = { "Filter Title",
		"Filter Description" };

ContentValues v = new ContentValues();
v.put("title", "New Title");
v.put("description", "New Description");

String where = "title='?' AND description='?'";

db.update("articles", v, where, whereArgs);
```

###delete()
The delete method works identical same as update, without the updated values parameter, as delete statements cannot target individual fields, but only entire rows.

###query()
There are several forms of this method, we'll focus on the one with eight parameters: table name, columns, WHERE clause, WHERE clause arguments, GROUP BY clause, HAVING clause, ORDER BY clause, and LIMIT clause. All of these clauses are formatted without the actual associated keyword. If you do not need a specific clause, pass in null. The query() method returns a Cursor object.

##Cursors
Cursors represent a grouping of data that was returned from a database query. They work like an iterator for moving forward or backwards through the returned data and have many useful functions that assist with navigating data. 

**getCount()** - return row count.
**get&lt;Type&gt;** - get a field value of the appropriate type.

```
// Query for all columns and rows in “articles”
Cursor c = db.query(“articles”, null, null, null, null, null, null, null);

if(c == null || c.getCount() == 0) {
	// No records
} else {
	c.moveToFirst();

	int id = c.getInt(0);
	String title = c.getString(1);
	String description = c.getString(2);
	String publishDate = c.getString(3);
	String url = c.getString(4);
}
```

###Displaying Cursor Data - ResourceCursorAdapter
There is a special type of adapter used to work with database cursors known as ResourceCursorAdapter. This adapter will accept a context, layout file, and a cursor. In order to use this adapter, you must override the bindView method to bind cursor data to an item view.  One of the nice things about this adapter is that the Cursor is advanced for you and so is the cursor count.  The data shown in the list can be updated by calling swapCursor followed by notifyDataSetChanged.  Note that the Cursor must contain an ID field or the adapter will not work.

```
class MyCursorAdapter extends ResourceCursorAdapter {

	public MyCursorAdapter(Context c, Cursor cur) {
		super(c, R.layout.simple_list_item_1, cur, 0);
	}

	@Override
	public void bindView(View v, Context c, Cursor cur) {
		String title = cur.getString(1);
		((TextView)v).setText(title);
	}
}
```

####References
http://developer.android.com/training/basics/data-storage/databases.html
