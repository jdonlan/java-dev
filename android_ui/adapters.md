#Adapters
All AdapterView view groups, such as Lists, Spinners, and Grids are populated with child views via an adapter. An AdapterView is a type of view group that, instead of creating the full layout via an XML file, creates and displays its child views based on a collection of data. Data collections, however, are just data. There is no visual representation that is innately part of a collection of data. In order to translate this data into a representative visual control, we use what's called an adapter.

Adapters are a middleman element that sits between a collection of data and an AdapterView. Adapters take in a collection of data and creates views that represent that data. Those views are then laid out within an AdapterView according to rules defined in a given control. Think back to the MVC pattern we discussed earlier this month. In this case, your data collection is the model, the AdapterView is the view, and the adapter is the controller. Let's take a look at how these pieces all work together.

##How Adapters Work
In the sections for the various AdapterViews, it was shown that an adapter takes in a collection of data, then the adapter is attached to an AdapterView. From there, the adapter handles the rest. So what is the adapter actually doing behind the scenes? To explain this, we'll use a ListView example...

When an adapter is set to a ListView, the first thing the list does is ask how many views it needs to create. To find this out, the adapter looks at the data collection and finds out how many elements are in the collection. Then the adapter tells the ListView how many items are in the collection. The ListView then starts asking for row layouts for the first few rows. The adapter then looks at the data in the collection that corresponds to the row. So if the ListView requests the first row layout, the adapter looks at the first element in the data collection. The adapter then takes that first element in the collection and creates a view that represents that data. After it creates the view, the adapter gives that view to the ListView to display as a row. The ListView and adapter do this over and over until all visible rows are created and shown.

After all of the visible rows are created and shown, the ListView becomes functional and the user can start scrolling through the list. When the user starts to scroll, the first row in the list will eventually be pushed off the screen and a new row will show at the bottom. At this time, the ListView gives the first row layout back to the adapter since it doesn't need it anymore and requests a new row layout to represent the newly visible row at the bottom. The adapter will then either recycle the view that it got back from the list or create a new row layout and return that to the ListView to be shown as the new row. 

A visual representation of this can be seen in the Google Developers video found at [https://www.youtube.com/watch?v=N6YdwzAvwOA](https://www.youtube.com/watch?v=N6YdwzAvwOA).

##The BaseAdapter Class
At the core of almost every adapter is the BaseAdapter class. The BaseAdapter class contains the base functionality (hence the name) that almost every adapter needs to implement. To better understand how adapters work, it's best to understand the methods contained within the BaseAdapter class.

###Example Empty BaseAdapter Implementation
```
public class ExampleAdapter extends BaseAdapter {
	public ExampleAdapter() {
	}
	@Override
	public int getCount() {
		return 0;
	}
	@Override
	public Object getItem(int _position) {
		return null;
	}
	@Override
	public long getItemId(int _position) {
		return 0;
	}
	@Override
	public View getView(int _position, View _convertView, ViewGroup _parent) {
		return _convertView;
	}
}
```

###public int getCount()
The getCount() method in an adapter returns how many items are stored in the underlying data collection. Typically, you would have some sort of array or ArrayList of data that your adapter is handling. In this method, you would return the length or size of that collection in this method. When the attached AdapterView asks for the size of the collection, this method is called.

###public Object getItem(int _position)
The getItem() method in an adapter is used to retrieve an item from the underlying data collection. The return type for this method is Object since the Object class is the superclass for all objects in Java. This means that you can swap this out with any other object type and the method would still work. You should change the return type of this method from Object to be whatever type of data is in your collection. If your collection is full of strings, change it to be public String getItem(int _position) instead. The position parameter being passed in represents the index in the collection that needs to be retrieved. This position is guaranteed to be a valid index in the collection so long as the getCount() method is filled out properly. This method is used in the getView() method (seen below) to get objects from the data collection in an index-safe manner.

###public long getItemId(int _position)
Since every view should have an ID to make it easy to reference, this method returns an ID for each view returned by this adapter. The position passed in is the position of the view in the AdapterView. Since each view should have a unique ID, you could just return the position as the ID. However, doing this leads to a problem if you have more than one AdapterView on screen at a time as the first view in each AdapterView would have an ID of 0. Since it's uncommon to refer to AdapterView items by ID, this is acceptable. However, if you want everything to be unique, you can always create a static constant in your adapter and add that to the row position to assign an ID. An example of this can be seen below.

```
private static final long ID_CONSTANT = 0x01000000;
...
public long getItemId(int _position) {
	return ID_CONSTANT + _position;
}
```

###public View getView(int _position, View _convertView, ViewGroup _parent)
The getView() method is where all the magic happens. This is the method that takes an object from a data collection and translates that object into a view to be displayed in the attached AdapterView. The position parameter here is the index of the item being requested by the AdapterView. Using that position, we can call getItem() to get the object from the underlying collection. Then we would create a view that represents that object, populate it with data, and return it. After returning a view from this method, the AdapterView receives that view and adds it to the layout on screen.

The getView() method, in addition to creating new views to show in the AdapterView, does something extra. You'll notice that there is a parameter for this method called _convertView of the type View. When scrolling through an AdapterView, it was mentioned that the AdapterView will give views back to the adapter when it's done with them. If the adapter has recycled views available, it will pass them to the getView() method to be reused. The convert view parameter is the recycled view being passed back to getView(). To use this recycled view, you'll have to make a simple conditional check. If the convert view is null, you need to make a new view. If the convert view isn't null, just use the convert view. Recycling views like this helps to cut down on memory usage and also helps to speed up your app when scrolling through a large number of elements in an AdapterView.

###Example Completed BaseAdapter
```
public class ExampleAdapter extends BaseAdapter {
	private static final long ID_CONSTANT = 0x010101010L;
	private Context mContext;
	private ArrayList<Employee> mEmployees;
	// We take in a context and list of Employee objects.
	// The list is our backing collection and the context is used
	// to create new views in our getView() method.
	public ExampleAdapter(Context _context, ArrayList<Employee> _employees) {
		mContext = _context;
		mEmployees = _employees;
	}
	// Returning the number of objects in our collection.
	@Override
	public int getCount() {
		return mEmployees.size();
	}
	// Returning Employee objects from our collection.
	@Override
	public Employee getItem(int _position) {
		return mEmployees.get(_position);
	}
	// Adding our constant and position to create unique ID values.
	@Override
	public long getItemId(int _position) {
		return ID_CONSTANT + _position;
	}
	@Override
	public View getView(int _position, View _convertView, ViewGroup _parent) {
		// If we don't have a recycled view, create a new view.
		if(_convertView == null) {
			// Creating the new view.
			_convertView = LayoutInflater.from(mContext).inflate(R.layout.example_list_item,, _parent, false);
		}
		// Get the object from the collection in an index-safe manner.
		Employee employee = getItem(_position);
		// Use the object from the collection to fill out our view.
		TextView tv = (TextView)_convertView.findViewById(R.id.employee_name);
		tv.setText(employee.getName());
		tv = (TextView)_convertView.findViewById(R.id.employee_department);
		tv.setText(employee.getDepartment());
		tv = (TextView)_convertView.findViewById(R.id.employee_service_time);
		tv.setText(mContext.getString(R.string.years, employee.getYearsOfService()));
		// Returning our filled out view.
		return _convertView;
	}
}
```

##Adapater Types
In addition to being able to create your own adapter using the BaseAdapter class, the Android SDK includes a few pre-built adapters that can be used for a variety of different things. There are adapters that are used for simple text content as well as adapters that communicate with database resources. We'll cover two of the commonly used pre-built adapters.

###ArrayAdapter&lt;T&gt;
An ArrayAdapter is a generic class that can be used with any type of object that has a string representation. Every object in Java extends the Object class as a base, therefore all objects have the toString() method available to them to override. The ArrayAdapter class takes in an array or list of objects and displays them using their string representation returned by toString().

When creating a simple ArrayAdapter, you must pass in at least a context and a layout resource that contains only a TextView. The context is used to create new views and the layout resource is the view that gets created. The ArrayAdapter also accepts an array or list collection of objects as one of the constructor overloads or it can accept just a list using the addAll() method. The adapter then takes the string representation of each object in the collection and sets it to the TextView resource passed into the constructor.

Since ArrayAdapter uses the toString() method of an object to show data, this adapter is not suited to showing a complex set of data. However, this adapter is great for showing single line list or spinner items. If you want to show more complex data, you'll need to create your own BaseAdapter implementation or, possibly, use a SimpleAdapter.

###SimpleAdapter
A SimpleAdapter is, as the name implies, very simple to use. However SimpleAdapter is, contrary to the name, not so simple to setup. A SimpleAdapter can be used in place of a custom adapter whenever you want to show more complex AdapterView items, but the views only contain text. If you want to create list, grid, or spinner items that have images in them, this is not the right adapter to use.

To use a SimpleAdapter, you must first convert each object into a HashMap&lt;String, String&gt;. This means mapping each point of data in your object to a string key that can represent it. After converting your objects into HashMaps, you would then add all of those HashMaps into an ArrayList&lt;HashMap&lt;String, String&gt;&gt;. Then you would need to create a string array of all your keys and an integer array of all of the view IDs where the data should be shown. Then you pass all of that data, including the list/grid item layout into the adapter constructor. Once you've done that, everything should just work. Behind the scenes, the getView() method is using the string array to pull data from the passed in HashMaps. Then getView() assigns that data from the HashMap to the TextView specified in the integer array.


