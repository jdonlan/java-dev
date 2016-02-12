#Collection Widgets
Collection widgets are used to show a list, grid, or stack of data. Creating a collection widget is very similar to creating any other widget, with a few additions of course. On top of the elements needed for a regular information or control widget, we need a few extra components to show our collection.

##Collection Widget Components

###RemoteViewsFactory
A RemoteViewsFactory is a special type of adapter-like class used to populate an AdapterView inside of a collection widget. This interface has methods that are very similar to adapters such as getCount() to return the number of items in the collection, and getViewAt() which is similar to getView() in an adapter, just with remote views instead of a regular view. All adapter functionality would take place in this class.

###RemoteViewsService
The RemoteViewsService is a type of service that is used specifically to generate a RemoteViewsFactory and connect it to a collection widget. There is only one required method override, onGetViewFactory(), and all you need to do in this method is return a RemoteViewsFactory. Also, because this is a type of service, you must register this service in the manifest in order for it to be started. When registering this component in the manifest, be sure to give this service the "android.permission.BIND_REMOTEVIEWS" permission so that it can bind to your collection view in order to supply the returned view factory.

```
<service android:name=".YourRemoteViewsService"
    android:permission="android.permission.BIND_REMOTEVIEWS" />
```
###PendingIntent Template
Normally, when setting click events for remote views, you'd use a pending intent for each different view that you wanted to click on. However, this behavior isn't allowed for collection views. Typically, a pending intent is used to contain an action that a user is likely to take from an outside component. It's a common behavior for a user to click on an item in a collection view, but it's not common for them to click on every single item in the collection. Therefore, the collection defines a single pending intent template which can then be customized for each item in the view using a fill-in intent.

###Fill-in Intent
A fill-in intent is not a pending intent, but rather a regular old intent. A fill-in intent should be created for each item in your collection view and should contain the same extras keys as your pending intent template. If the extras keys aren't the same, the fill-in intent will not work. The exception to this would be if the template doesn't have any extras. If no extras are set in the template, you can use whichever extras you want in the fill-in intent. The fill-in intent should contain whatever data is needed to identify the individual row in the collection as if you were going to handle it in an OnItemClickListener.

##Implementating a Collection Widget
Now that we've covered the different pieces of a collection widget, let's take a look at the actual implementation. The first thing we'll need to create is the widget layout. The only difference between a standard widget and a collection widget in this regard, is the type of views used in the layout. For a collection widget, you must specify either a ListView, GridView, StackView, or AdapterViewFlipper in the layout. Additionally, you can set a view to show when that collection view is empty. This can be any type of non-adapter based view. Be sure to assign each an ID to reference them later on.

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical"
    android:background="@android:color/background_dark" >
    <ListView android:id="@+id/article_list"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
    
    <TextView android:id="@+id/empty"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:text="@string/empty" />
</LinearLayout>
```

The rest of this demo is going to jump around since several steps need to be completed to get the widget to work, but they don't have to happen in any particular order. For our next step, we'll create a new class that implements the RemoteViewsFactory interface. We're going to need a context in this class so let's take one in with our constructor.

Once you implement all the methods in the interface, there are a few that we need to take a look at. The first method is onCreate(). The onCreate() method is called when the factory is first created. This is where you would setup any initial data to show in the collection. This method can handle some minor heavy lifting but, if you need to load a lot of data from the network, then do that in the onDataSetChanged() method. The next two methods we need to take care of are getViewTypeCount() and hasStableIds(). The getViewTypeCount() method returns the number of different view types used in the collection. We'll only be using one view type, so make this method return 1. Also, all of our row items are going to have stable IDs, so make hasStableIds() return true.

Once that's done, we can move on to the adapter like methods that get implemented to show data in the collection. The first is getCount(). This works exactly the same as getCount() in an adapter and should be used to return the number of items in the collection. Then there's the getViewAt() method that works like getView() in an adapter. in this method, we would create a new RemoteViews object that represents our row item, setup any data that needs to be setup, and return the remote view. We can also setup our fill-in intent here and attach it to the returned view. For this example we'll be showing a list of fake news articles so that's reflected in the code below.

```
public class CollectionWidgetViewFactory implements RemoteViewsFactory {
	
	private static final int ID_CONSTANT = 0x0101010;
	
	private ArrayList<NewsArticle> mArticles;
	private Context mContext;
	
	public CollectionWidgetViewFactory(Context context) {
		mContext = context;
		mArticles = new ArrayList<NewsArticle>();
	}
 
	@Override
	public void onCreate() {
		String[] titles = { "New Phones Released!", "Random App Makes $300 Million", 
				"Google Glass Robots", "Arduino Used in Mobile", "Orioles Win World Series" };
		String[] authors = { "Phone Scoop", "Yahoo Marketing Department", "Cyborg Alliance",
				"Some Open Source Person", "Roch Kubatko" };
		String[] dates = { "Everyday", "June 20, 2012", "September 10, 2014", "August 9, 2014", "November 10, 2014" };
		
		for(int i = 0; i < 5; i++) {
			mArticles.add(new NewsArticle(titles[i], authors[i], dates[i]));
		}
	}
 
	@Override
	public int getCount() {
		return mArticles.size();
	}
 
	@Override
	public long getItemId(int position) {
		return ID_CONSTANT + position;
	}
 
	@Override
	public RemoteViews getLoadingView() {
		return null;
	}
 
	@Override
	public RemoteViews getViewAt(int position) {
		
		NewsArticle article = mArticles.get(position);
		
		RemoteViews itemView = new RemoteViews(mContext.getPackageName(), R.layout.article_item);
		
		itemView.setTextViewText(R.id.title, article.getTitle());
		itemView.setTextViewText(R.id.author, article.getAuthor());
		itemView.setTextViewText(R.id.date, article.getDate());
		
		Intent intent = new Intent();
		intent.putExtra(CollectionWidgetProvider.EXTRA_ITEM, article);
		itemView.setOnClickFillInIntent(R.id.article_item, intent);
		
		return itemView;
	}
 
	@Override
	public int getViewTypeCount() {
		return 1;
	}
 
	@Override
	public boolean hasStableIds() {
		return true;
	}
 
	@Override
	public void onDataSetChanged() {
		// Heavy lifting code can go here without blocking the UI.
		// You would update the data in your collection here as well.
	}
 
	@Override
	public void onDestroy() {
		mArticles.clear();
	}
	
}
```

Now that our factory is setup, we'll need a RemoteViewsService in order to attach the factory to the collection widget. This is probably the easiest step in the whole process as we only need to create a class that extends RemoteViewsService, override onGetViewFactory() to return our factory, and register the service in the manifest with the proper permission. Example manifest tag can be seen above in the components section.

```
public class CollectionWidgetService extends RemoteViewsService {
	@Override
	public RemoteViewsFactory onGetViewFactory(Intent intent) {
		return new CollectionWidgetViewFactory(getApplicationContext());
	}
}
```

The last thing we need to do is setup our AppWidgetProvider to bind all of our components together. We do this by creating a new AppWidgetProvider and overriding both onUpdate() and onReceive(). In the onUpdate() method, we do our normal widget setup, but we also bind our service to the widget's view using a service intent and the setRemoteAdapter() method of the RemoteViews class. We also specify which of our widget views is going to be the empty view. Then, we create and set our pending intent template. For our example, we've created a custom broadcast action that will fire when our items are clicked. We create an intent using that action, set it to a broadcast PendingIntent object, and set that to be our template. Then we update the widget as usual.

In the onReceive() method, we can now listen for that custom broadcast action we assigned in our pending intent template. If we receive an intent of that type, we can do whatever we want with it. If it's not the custom action, be sure to call super so that onUpdate gets called when necessary. Don't forget to register this receiver in the manifest to listen for both the update and custom intent actions.

```
public class CollectionWidgetProvider extends AppWidgetProvider {
	
	public static final String ACTION_VIEW_DETAILS =
		"com.company.android.ACTION_VIEW_DETAILS";
	public static final String EXTRA_ITEM =
		"com.company.android.CollectionWidgetProvider.EXTRA_ITEM";
	
	@Override
	public void onUpdate(Context context, AppWidgetManager appWidgetManager,
			int[] appWidgetIds) {
		
		for(int i = 0; i < appWidgetIds.length; i++) {
			
			int widgetId = appWidgetIds[i];
			
			Intent intent = new Intent(context, CollectionWidgetService.class);
			intent.putExtra(AppWidgetManager.EXTRA_APPWIDGET_ID, widgetId);
			
			RemoteViews widgetView = new RemoteViews(context.getPackageName(), R.layout.widget_layout);
			widgetView.setRemoteAdapter(R.id.article_list, intent);
			widgetView.setEmptyView(R.id.article_list, R.id.empty);
			
			Intent detailIntent = new Intent(ACTION_VIEW_DETAILS);
			PendingIntent pIntent = PendingIntent.getBroadcast(context, 0, detailIntent, PendingIntent.FLAG_UPDATE_CURRENT);
			widgetView.setPendingIntentTemplate(R.id.article_list, pIntent);
			
			appWidgetManager.updateAppWidget(widgetId, widgetView);
		}
		
		super.onUpdate(context, appWidgetManager, appWidgetIds);
	}
	
	@Override
	public void onReceive(Context context, Intent intent) {
		
		if(intent.getAction().equals(ACTION_VIEW_DETAILS)) {
			NewsArticle article = (NewsArticle)intent.getSerializableExtra(EXTRA_ITEM);
			if(article != null) {
				// Handle the click here.
				// Maybe start a details activity?
				// Maybe consider using an Activity PendingIntent instead of a Broadcast?
			}
		}
		
		super.onReceive(context, intent);
	}
}
```

If you've followed along with all of the steps properly, your collection widget should be showing up with a list of five articles. There are lots of little things that could go wrong in building a collection widget so make sure that, in addition to following along with this process, you also download and dig through the supplied example project. Try altering the widget UI to also have control buttons. Maybe make the data in the list dynamic and try updating the list. 