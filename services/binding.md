#Binding to Services
Services are a great way to run operations in the background long after an application closes. However, while your app is open, you might want to directly access a service to call methods or influence how it runs. Unfortunately, the only way to interact with a started service is to create a new intent and call startService() again. While this is great generic messaging and communication, it is not so great if you want to interact with the service directly. In order to interact with a service directly (like to control audio playing in a service), we need to bind to it.

##How to Bind to a Service
If you recall in our services example, we had to override an onBind() method in our service and that method just returned null. If we want to bind to a service, we need to make this onBind() method return a Binder object that can be used to communicate with the service. A binder is an object that's used for accessing a service and its methods outside of the actual service class. Binders only work if the bound service is private to your application and exists within the same process. Since all of our services fit within those parameters, they're good candidates for using a binder.

To implement a binder, the first thing we need to do is create a public inner class in our service that extends Binder. Then, we can add whatever methods we want to that binder to access the service. The easiest way to do this is to create a single method in your binder class that simply returns an instance of the containing service. This allows any binding activities to directly access the service using a Service object. Then, we just return an instance of that binder in the onBind() method. We'll also set up a public method in our service to show a toast so we can interact with that later.

```
public class BoundService extends Service {
	
	public class BoundServiceBinder extends Binder {
		public BoundService getService() {
			return BoundService.this;
		}
	}
	@Override
	public IBinder onBind(Intent intent) {
		return new BoundServiceBinder();
	}
 
	public void showToast() {
		Toast.makeText(this, "Some text!", Toast.LENGTH_SHORT).show();
	}
}
```

That's it, our service is ready for binding. You should never bind to a service if the onBind() method returns null. The system will allow you to bind to that service but, without a binder, you can't really do anything that you couldn't already do without binding. Now that our service supports binding, let's take a look at how to actually bind to that service.

The first thing we need to do is implement the ServiceConnection interface in the activity that is doing the binding. The ServiceConnection interface contains two callbacks that are used to relay information about the connection with the bound service. The first method, onServiceConnected(), is called when the service becomes bound. This method also contains an IBinder object which represents the binder we returned in our interface. The other method, onServiceDisconnected(), is used to inform the binding activity that the service has crashed or has been killed by the system and that the service is no longer bound. Let's start by implementing the interface.

```
public class MainActivity extends Activity implements ServiceConnection {
 
	@Override
	public void onServiceConnected(ComponentName name, IBinder service) {
		
	}
 
	@Override
	public void onServiceDisconnected(ComponentName name) {
		
	}
	
}
```

Once we've implemented our interface, we can now bind to the service and get our binder and service objects. To do this, we call the bindService() method and pass in the same intent we would use to start a service along with a reference to the ServiceConnection and a constant which tells the system to create the service if it doesn't exist and don't let it die until we unbind. Binding to the service happens asynchronously so don't treat your service as bound immediately after calling bindService(). You'll need to wait until the onServiceConnected() callback fires before you can ensure the binding is made. Inside of the onServiceConnected() callback, you can cast the IBinder object to your binder type and call any methods you wish to call on the binder or the returned service.

```
public class MainActivity extends Activity implements ServiceConnection {
 
	@Override
	protected void onStart() {
		super.onStart();
 
		Intent intent = new Intent(this, BoundService.class);
		bindService(intent, this, Context.BIND_AUTO_CREATE);
	}
	
	@Override
	protected void onStop() {
		super.onStop();
	
		unbindService(this);
	}
 
	@Override
	public void onServiceConnected(ComponentName name, IBinder service) {
		BoundServiceBinder binder = (BoundServiceBinder)service;
		BoundService myService = binder.getService();
		myService.showToast();
	}
 
	@Override
	public void onServiceDisconnected(ComponentName name) {
		
	}
	
}
```

It's important to note that you should not attempt to unbind from a service if you're not already bound as attempting to do so will throw an IllegalArgumentException. Also, since bound services can't stop while bound, don't try calling stopService() while your service is bound as nothing will happen and it'll be hard to debug.

##About Bound Services
Now that you've seen how they work, let's talk about a few odd behaviors in dealing with bound services. The first thing is, you don't have to explicitly start a service before binding to it. If you attempt to bind to a service that hasn't been started, the system will start the service for you. Then, when you unbind from the service, the service will stop itself. However, if you start a service, then bind to it, you cannot stop the service unless you unbind. So if you start, bind, then attempt to stop, nothing will happen. However, when you unbind from that service, the stop command will then be processed and the service will stop. If you were to start a service, bind, and unbind without calling stop, the service would continue running until it was stopped. So to recap:

* Unbinding will stop bound services that haven't been started.
* If you start a service, you must call stop on it to stop it.
* If you start a service and bind to it, you cannot stop that service until you have unbinded.

Lastly, if you unbind from a service, you still have access to the Binder and Service objects that were returned during binding. This is because there's no callback to say if a service unbinded successfully. Be sure that when you unbind from a service, you null out your binders and services so that you don't encounter any bugs from the service being destroyed behind the scenes, and also so you don't hold on to that memory and stop it from being cleaned up by the system properly.