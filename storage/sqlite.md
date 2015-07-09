#SQLite

##The SQLite Language
Data is organized into two-dimensional tables of rows and columns. Columns define the type of being stored and what that data represents. Rows represent a single data entry that is composed of the data points represented by the columns. 

###Primary Keys
Every table should contain a column which holds a unique identifier for each row in the table known as a Primary key.  In general, these keys are represented by an integer value that auto increments when a new row is added to the table.

###Data Types
* TEXT - Any sort of string and/or character data.
* INTEGER - A whole number represented by an int, short, byte, or long.
* REAL - A floating point number represented by a float or a double.
* DATETIME - A string that represents a date formatted as “yyyy-MM-dd HH:mm”.
* BLOB - Raw byte data stored as a collection of bytes.
 
####Example Table
![](sqlite.png)

###Creating a Table
Tables are creating using a “CREATE TABLE” statement and are named using a string value that does not contain spaces. The table name is followed by the column names and types, separated by commas, and listed within parentheses.

Create a table of articles that stores the title, description, publish date, and the URL of the story.

```
CREATE TABLE IF NOT EXISTS articles (
	_id INTEGER PRIMARY KEY AUTOINCREMENT,
	title TEXT,
	description TEXT,
	publish_date DATETIME,
	url_string TEXT
)
```

###Querying a Table (SELECT)
Queries are performed using a SELECT statement. The SELECT statement is formatted as the command SELECT, followed by the data fields (columns) in a comma separated list, followed by the table or tables that house the data.

Assuming a table named “articles” and we want to fetch all article titles from a column named “title”:

```
SELECT title FROM articles
```

####Filter the Selection (WHERE)

A SELECT statement will return all rows in a table by default. Returned rows can be filtered using a WHERE clause. WHERE statements are formatted as the command WHERE, then column name(s) a comparison operator, then column value(s).  This WHERE clause is placed after the SELECT statement as the statement tail. String values are wrapped in quotes while numerical values contain no quotes.

To get all articles named “test” from the table above:
```
SELECT title FROM articles WHERE title=‘test’
```

The WHERE clause can also be filtered using a wildcard (%) and the LIKE keyword for string values. Standard <, >, <=, >=, != operators can be used for numeric values. != works for strings as well. Can combine multiple WHERE clauses with AND/OR keywords. 

To get all articles starting with the word “test” or containing the word “example”:
```
SELECT title FROM articles WHERE title LIKE ‘test%’ OR title LIKE ‘%example%’
```
####Sorting Query Results
Query results can be sorted by adding an ORDER BY statement to the end of the SELECT statement, after the WHERE clause if included. ORDER BY statements are followed by the column name and either ASC or DESC for ascending or descending order. Only  INTEGER, TEXT, or REAL columns should be used for sorting to get a reliable sort behavior.  

Sorting all articles by title name in ascending order:
```
SELECT title FROM articles ORDER BY title ASC
```

####Limiting Query Results

After filtering and sorting a SELECT query, you may find that you have more results than appropriate for your needs.  SQLite supports limiting the number of results using the LIMIT modifier.

Getting the top 5 articles starting with the word "test" in alphabetical order:
```
SELECT title FROM articles WHERE title LIKE ‘test%’ ORDER BY title ASC LIMIT 5
```

###Writing to a Table
Data is added to a table as a new row using an INSERT INTO statement.  This statement is formatted as INSERT INTO, followed by the table name, a comma separated list of field names surrounded by parenthesis, the word VALUES, followed by a comma separated list of values that are respective to the field list provided. If a field is a required data piece of the table, it must be included in the list of field names and have a corresponding value.  If a field is optional it can be omitted.  The one exception to this rule is fields that are required but are automatically generated, such as in the case of an automatically incrementing primary key.  Attempting to force a value in a field such as this can cause errors and data corruption.

Insert a new article into our articles table:
```
INSERT INTO articles (title, description) VALUES (‘Example Title’, ‘Example Description’)
```

###Updating Table Data
Data can be updated using an UPDATE statement. Statement is formatted as UPDATE, followed by the table name, the word SET, followed by a comma separated list of key value pairs for field names and values, and finally a WHERE condition formatted as it would in a query.  It should be noted that the WHERE clause is optional but if no WHERE clause is provided then all rows are updated.

Update the description of our example article in the articles table:
```
UPDATE articles SET description=‘Changed’ WHERE title=‘Example Title’
```

###Deleting Table Data
Data is deleted from a table using a DELETE statement. The statement is formatted starting with the command DELETE FROM, followed by the table name, and finally a WHERE condition.  You'll notice that the field names are omitted from this command.  This is because only entire rows can be deleted.  If you want to set only some fields in a row to empty, you must use an UPDATE command.  As with the UPDATE command, the WHERE clause is optional and not providing a WHERE clause will delete all records in the table.

Delete our example article from the articles table:
```
DELETE FROM articles WHERE title=‘Example Title’
```

##SQLite in Android
