#Sensors

Android devices have a multitude of different sensors that are available to the developer for integration in their apps. One of the benefits of making apps for mobile devices is the ability to use data from the multitude of sensors embedded into the device. While each of these sensors may be used to return different data or serve a different purpose, we can interact with all of these sensors in a very standard way. The simplest sensor to use is the ambient light sensor, so we'll use that as our basis for working with sensor data.

The first thing we would need to do to start working with sensor data is to get a SensorManager from the system. We can do this the same way we would any other system service, by calling getSystemService() on a context and asking for the SENSOR_SERVICE.

```
// Assuming we're in a context.
SensorManager mgr = (SensorManager) getSystemService(SENSOR_SERVICE);
```

Now that we have our manager, we can start accessing our sensors. Every device has a different set of available sensors and not every sensor is available on every device. This is especially true with Android TV and Android Auto where you wouldn't find typical phone sensors such as the accelerometer or proximity sensor. Because of this, you need to make sure that when you request a sensor that it exists. Never assume that all devices will have a particular sensor just because one or two devices has it.

When attempting to access sensors, there are two ways to go about requesting access. The first is to request all sensors of a given type. To do this, you can call getSensorList() and pass in the type of sensor that you wish to get using one of the many sensor type constants in the Sensor class. This method will return a list of Sensor objects which you can cycle through and find the desired sensor. The other way to request a sensor is to just fetch the default sensor for a given type. It's possible to have multiple sensors for a given type so if you just want one sensor, request the default. You can do this by calling the getDefaultSensor() method and passing in a sensor type constant.

```
// Get all light sensors
List<Sensor> sensors = mgr.getSensorList(Sensor.TYPE_LIGHT);
 
// Get the default light sensor
Sensor lightSensor = mgr.getDefaultSensor(Sensor.TYPE_LIGHT);
 
// Check if sensor exists
if(lightSensor != null) {
	// Light sensor exists, we can use it here.
} else {
	// Device doesn't have an ambient light sensor.
}
```

Once we have our sensor, we need to setup a SensorEventListener in order to register for updates and start getting sensor data. SensorEventListener is an interface, so we can implement this in our activity or fragment or in a completely separate class if we want. For this example, we'll assume it's implemented by our activity and we'll just show the overridden methods.

The two methods we need to override for our event listener are onAccuracyChanged() and onSensorChanged(). The onAccuracyChanged() method is called whenever the accuracy of the provided sensor changes, for better or worse. The accuracy value will be either low, medium, or high based on constants provided by the SensorManager class so feel free to check this value if you only want readings of a particular accuracy. Since we don't care too much for accuracy and only want the raw data, we'll focus mainly on the onSensorChanged() method. This method takes in a single SensorEvent object which contains a few properties we can access. The first of these is the sensor which generated the event in the form of a Sensor object. We also have a timestamp of when the event was generated and an integer that specifies the accuracy of the event. Then there's the most important property, an array of floats named "values".

The values array contains the raw sensor values output by the registered sensor. This array has different values depending on the sensor that was registered and is also a different length for each sensor. A full listing of all values for each sensor can be found in the SensorEvent documentation. For the light sensor, we only care about the first value in the array which is the lux value of the ambient light. This can be any value from 0 to over 10,000 depending on the light levels. This can also change based on the type of light (artificial vs natural).

```
@Override
public void onAccuracyChanged(Sensor sensor, int accuracy) {
	// Check accuracy value here if needed.
}
	
@Override
public void onSensorChanged(SensorEvent event) {
	// Make sure this is a light sensor.
	if(event.sensor.getType() == Sensor.TYPE_LIGHT) {
		long eventTime = event.timestamp;
		int accuracy = event.accuracy;
	
		float lux = event.values[0];
		// Use value in application...
	}
}
```

Now that we have our listener setup, we can start requesting sensor data updates. We can do this using the registerListener() method of the sensor manager. It's important to note that running sensor updates can consume a fair amount of battery life. Because of this, you want to make sure you're only running updates when absolutely necessary. If the updates are only needed in a single activity, make sure you register in onResume() and unregister in onPause(). This will help cut down on battery drain. Also, make sure you're only requesting updates at the speed that they're needed. There are four speeds you can register: normal, UI, game, and fastest. Normal update speed should be suitable for most applications. However, if you're building a game then you'd want to use the game update speed. If your UI moves and/or updates based on sensor data, you'd use the UI update speed. The fastest update speed is almost never needed and shouldn't be used unless you're writing a profiling tool.

```
SensorManager mgr = (SensorManager)getSystemService(SENSOR_SERVICE);
	
Sensor lightSensor = mgr.getDefaultSensor(Sensor.TYPE_LIGHT);
if(lightSensor != null) {
	mgr.registerListener(this, lightSensor, SensorManager.SENSOR_DELAY_NORMAL);
}
```

####References
http://developer.android.com/guide/topics/sensors/sensors_overview.html
http://developer.android.com/reference/android/hardware/SensorManager.html
http://developer.android.com/reference/android/hardware/SensorEvent.html