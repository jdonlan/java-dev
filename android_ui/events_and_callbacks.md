#Events and Callbacks
Mobile development is about interacting with users to simplify or enhance their lives. As such, a key to any successful mobile application is human computer interaction - responding to user input and handling them within the application. This section will introduce you to some of the basic Android events and callbacks which will allow the application to respond to user interaction.

Events and Callbacks are the primary components of handling user interaction with application code. Regardless of the interaction event, the basic flow is the same...

1. Application registers listeners for view controls
2. Application presents the user with the controls
3. User interacts with a control
4. Application detects the interaction and accesses the callback
5. Code registered in the callback is executed

While it seems complicated, the process is actually straight forward. Once the UI is built and designed, it is simply a matter of telling the application which controls should respond to user interactions and what behaviors it should take in response to these interactions.

Android events take on many forms, depending on the platform and device which is running the application, but typically follow common naming conventions and types. Events can be handled for various types of activities, such as user input, hardware responses (such as GPS or accelerometer changes), network interactions (such as download progress), and so on.

##Event Listeners
Event Listeners attach to views within an application signifying that the application should respond to user interaction. For example, a button within an application must listen for a user to press it so that it knows it needs to respond to said press. As such, the application must attach an *OnClickListener* to the button to do so. These listeners contain a single callback which will be fired when the event is registered with the system. In the case of the *OnClickListener*, for example, the *onClick* callback is triggered, allowing the desired code to run in response.

###Example

Attaching an OnClickListener to a Button myButton

```
protected void onCreate(Bundle savedInstanceState) {
        ...
        Button button = (Button)findViewById(R.id.myButton);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //CODE
            }
        });
        ...
}
```

##Event Handlers
Event Handlers, like Event Listeners, allow the application to run callback methods in response to system events. These events aren't in response to a view, but typically system interactions that are fired more generically such as those involving system hardware like the GPS or accelerometer, touch events including the keyboard or other non-view UI controls, or application flow such as focus changes, animations, or view loads.

####References
http://developer.android.com/guide/topics/ui/ui-events.html
