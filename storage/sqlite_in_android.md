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

The delete method works the same as update, minus the updated values parameter.