#Basic Layouts

##LinearLayout
The linear layout is used to position items in a stack or a row. When using a linear layout, you would first set the orientation to be horizontal or vertical. After setting the orientation, any views that are added to this layout will be stacked one on top of the other (top to bottom) in vertical mode, or positioned right next to each other (left to right) in horizontal mode.

Linear layouts are well suited to creating simple forms or for arranging other layouts in a straight line. It should also be noted that since linear layouts have a strict horizontal or vertical positioning system, they are the most efficient type of layout. This is because the system knows that each view will be in a stack or row and can wait until all views are added before measuring and positioning views on the screen.

##RelativeLayout
The relative layout is used to position items on the screen relative to the size of the container and the position of other views. When adding views to a linear layout, you know that the view you add will be right below or just to the right of the previous view. In a relative layout, all views will default to being in the top left corner of the screen. To change view positions, you would set their positioning relative to other views. For instance, if you want the first view in the center of the screen, you'd set its position to be in the center of the relative layout. If you wanted the next view to be below the one in the center, you'd set the view to align itself below the first view. Each view needs to have its relative position specified or else it shows up in the top left. Relative layouts are great for making complex UIs that need to have certain elements in certain places, no matter what the screen size. 

For instance, if you want one view in each corner of the screen, no matter the screen size, you would add four views and assign each to a corner of the layout. Then if the view size changes for different devices, the views still stay in the corners. Because each view is positioned relative to the container and other views, it is possible that UI elements could overlap each other. That makes this layout a bad choice for any screen with data entry as you might end up blocking input controls on smaller screen devices. 

Because each view is positioned relative to the next, the system needs to measure and draw every view each time a new view is added, and it needs to do this all over again if the device orientation or layout size changes. Because of the added work for the system, this is the least efficient type of layout to use and should only be used when absolutely necessary.

##TableLayout
The table layout, much like the name implies, is used to position views in a table. When creating a table layout, you can specify the number of rows and columns and position views into specific table cells. Like HTML, views can span multiple rows and/or columns and the size of each cell can adjust to the controls that are added to the layout.

Table layouts add an extra level of horizontal and vertical positioning over the very simple linear layout and, because of the strict row/column layout, are more efficient to use than relative layouts. Table layouts are the ideal layouts to use when creating more complex forms or non-uniform static grids. 

For uniform static grids there is the GridLayout and for non-uniform grids there is the GridView control.