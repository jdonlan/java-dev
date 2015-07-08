#Advanced Views - GridView
When showing a collection of items that have a unique visual representation, such as a collection of images, it's best to show these items in a grid to help emphasize that visual component.

While a ListView makes it very easy to show a collection of data in a uniform list and works really well for showing data that's text heavy, what if your data is more visual? A collection of images, for instance, is likely better represented by the image contents rather than the image name. To help show collections of visual content, Android provides the GridView control.

The GridView control is a type of AdapterView that shows content in a uniform grid. Where as a ListView can only show content in a single full width column, a GridView can use multiple columns with padding and spacing between each grid tile. Much like ListView, there are four components involved in creating a working grid: the GridView, the grid item layout, the backing data collection, and the adapter which binds it all together. The data collection is very much the same as the ListView, but we'll cover the other components below.

##Components

###GridView XML Resource
A GridView is defined in XML using the &lt;GridView /&gt; tag and specifying an ID, layout width, and layout height. As with lists, it's typical to have your grid stretch the full width and height of the layout. Additionally, there are a few other properties that you can set here. The numColumns property allows you to set how many columns your grid will contain. You don't set the number of rows since that's determined by the amount of data you have in your backing collection. The horizontalSpacing and verticalSpacing properties will allow you to define how much space you have between each tile in your grid. Likewise, you can set a fixed width for columns using the columnWidth property or you can set how columns stretch to fill any extra spacing using the stretechMode property. The below declaration will create a grid with two columns that are evenly spaced and distributed.

```
<GridView android:id="@+id/image_grid"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:numColumns="2"
    android:stretchMode="columnWidth" />
```

###Grid Item Layout
The grid item layout is the layout file that will determine what your grid items will look like. When creating this layout, it's important to keep the number of columns in your grid in mind. If your grid only has one column, you'll want to specify a wide layout. If your grid has many columns, you'll want to specify a more narrow layout. If you're going for a tile look, you'll want to make your grid items square. It's important to note that the same restrictions apply to grid items as they do to list items (no scrolling containers, keep text light, etc).

##Building a GridView