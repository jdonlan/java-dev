#System Broadcasts
The Android system is capable of sending several protected broadcasts that present information about changes in the device status. Intents using these actions cannot be broadcasted from within your app as they're reserved by the system for broadcasting changes in device status. While we can't send broadcasts of these types, it is still possible to setup our receivers to listen for these broadcasts. Below are a few of the system broadcasts, descriptions of what data they contain, and when you would use each.

##
31
JUL
STATUS
Overview
Downloads
Submit
Comments
Overview
The Android system is capable of sending several protected broadcasts that present information about changes in the device status. In this lesson, we'll talk about some of the different system broadcasts and how you can listen for those broadcasts in your applications.

Objectives & Outcomes
Upon completion of this lesson, the student should:
Be able to identify protected system broadcast events and what each is used for.
Instructions
As mentioned in a previous lesson, the Android SDK provides several protected broadcast intent actions. Intents using these actions cannot be broadcasted from within your app as they're reserved by the system for broadcasting changes in device status. While we can't send broadcasts of these types, it is still possible to setup our receivers to listen for these broadcasts. Below are a few of the system broadcasts, descriptions of what data they contain, and when you would use each.

##ACTION_TIME_TICK
The [ACTION_TIME_TICK](http://developer.android.com/reference/android/content/Intent.html#ACTION_TIME_TICK) broadcast intent is sent whenever the current time has changed on the device. This is only sent once per minute as to not kill a device's battery by firing every second. Additionally, apps can only register to receive this intent through a receiver that is registered in code. This intent cannot be applied to an intent filter that is declared in the manifest as that would cause the app to wake up once per minute, every minute, regardless of whether or not the app was running. This would be a huge drain on the battery so it has been disabled by the Android system. This intent would be used if you wanted to show the current time in your app and always have that time shown as up to date.

##ACTION_BOOT_COMPLETED
The [ACTION_BOOT_COMPLETED](http://developer.android.com/reference/android/content/Intent.html#ACTION_BOOT_COMPLETED) broadcast intent is sent whenever the device is first turned on and completes the boot process. Some apps, such as the Google Play Store, need to check for updated data or perform some actions whenever the device first starts up. For instance, if you were setting an alarm on the device, all alarms are cleared out by the system when the device turns off. When the device is powered back on, you should check if any alarms have passed and, for those that haven't passed, reschedule the alarms. By listening for the ACTION_BOOT_COMPLETED intent, you can do this right when the device starts and there's no interruption in the functionality of your app.

##ACTION_SHUTDOWN
The [ACTION_SHUTDOWN](http://developer.android.com/reference/android/content/Intent.html#ACTION_SHUTDOWN) broadcast intent is sent whenever the device is about to shutdown. When a user chooses to power down their device, or if the device powers itself down due to low battery, this intent will be fired. If your app is performing any sort of data input or downloads and you want to make sure that your data is saved before the device shuts down, it's a good idea to listen for this intent. By doing so, your receiver will fire when the device is about to shutdown and you'll have one last opportunity to save any unsaved data before shutdown completes. It's important to note that this intent only fires if the device shuts down in a safe manner. If the device crashes and forces a shutdown, or if the battery is pulled from the device, this intent will not have an opportunity to fire. To account for those situations, make sure you're saving your data often and to temporary files if needed before a hard save request has been made.

There are several other system broadcast events such as ACTION_PACKAGE_ADDED when new apps are installed, ACTION_PACKAGE_REMOVED when apps are uninstalled, ACTION_POWER_CONNECTED when the device has been plugged into a power source, and ACTION_BATTERY_CHANGED when the battery level on the device changes. Each of these intents have their own particular uses in different types of applications. Try experimenting with some of these intents to discover how each one works.