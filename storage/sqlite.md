#SQLite

##Data Tables
Data is organized into two-dimensional tables of rows and columns. Columns define the type of being stored and what that data represents. Rows represent a single data entry that is composed of the data points represented by the columns. 

###Primary Keys
Every table should contain a column which holds a unique identifier for each row in the table known as a Primary key.  A common convention is to trail the name of Primary Keys with “_id”. In general, these keys are represented by an integer value that auto increments when a new row is added to the table.

###Data Types
* TEXT - Any sort of string and/or character data.
* INTEGER - A whole number represented by an int, short, byte, or long.
* REAL - A floating point number represented by a float or a double.
* DATETIME - A string that represents a date formatted as “yyyy-MM-dd HH:mm”.
* BLOB - Raw byte data stored as a collection of bytes.
 
###Example Table
![](sqlite.png)

