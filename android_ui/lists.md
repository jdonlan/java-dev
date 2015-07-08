#ListView

A very common UI pattern for displaying a group of related data is to show that data in a vertically scrolling list. To achieve this type of list display, you would use a ListView. The Android ListView is a special type of view group known as an AdapterView. Unlike other view groups where the views contained within are all determined via a layout file, such is the case with LinearLayout, an AdapterView creates its views on the fly based off the data contained within an adapter.

When using a ListView in your app, there are four components that we need to focus on: the ListView which will be used to show the list of data, the list item layout which will represent each row in the list, the collection of data to show in the list, and the adapter which hooks everything up. We'll go into each of these below with the exception of the adapter since that will be covered more in a later lesson.

*Note: In this lesson, you'll see a class called ArrayAdapter being used. For more on this and other types of adapters, see the section on adapter events and callbacks. For now, think of adapters as factories that take in a collection of data and return a list of views that represent that data.*


