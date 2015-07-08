#Advanced Views - GridView
When showing a collection of items that have a unique visual representation, such as a collection of images, it's best to show these items in a grid to help emphasize that visual component.

While a ListView makes it very easy to show a collection of data in a uniform list and works really well for showing data that's text heavy, what if your data is more visual? A collection of images, for instance, is likely better represented by the image contents rather than the image name. To help show collections of visual content, Android provides the GridView control.

The GridView control is a type of AdapterView that shows content in a uniform grid. Where as a ListView can only show content in a single full width column, a GridView can use multiple columns with padding and spacing between each grid tile. Much like ListView, there are four components involved in creating a working grid: the GridView, the grid item layout, the backing data collection, and the adapter which binds it all together. The data collection is very much the same as the ListView, but we'll cover the other components below.

###GridView XML Resource
