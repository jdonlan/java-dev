#Connecting to APIs
Application Programming Interfaces, APIs for short, provide developers with programmatic access to external data and/or tools that can enhance an application. These interfaces establish rules which define the interaction between the remote source and the calling application. While each API is different, the general process for utilizing them to enhance an application is fundamentally the same.

1. Authenticate (optional - depending on API)
2. Structure Request
3. Make Request
4. Receive Response
5. Parse Response
6. Utilize Response

As APIs are typically remote, applications must make network calls to the remote API to receive the data. As such, it is important to remember to enable the proper permissions in Android to enable access to the internet:

```
<uses-permission android:name="android.permission.INTERNET" />
```

As these remote calls utilize the a URLConnection to connect to the internet and make the request, it is also important to remember the restrictions for doing so within Android - namely, you cannot make asynchronous requests on the main UI thread. As such, API calls are generally made on a separate thread, such as with an AsyncTask. Using the general process above, an Android API call using an AsyncTask would look as follows:

1. Structure Request - URLConnection
2. Make Request - AsyncTask.execute(url)
3. Receive Response - doInBackground()
4. Parse Response - onPostExecute()
5. Utilize Response - Custom Method

###Considerations
Note that AsyncTask is perfectly set up to handle a remote API call, providing all the needed methods for communicating with a remote source. That said, there are some considerations that should be considered when working with remote data:

**Data Changes** - Whenever working with a 3rd party, it is important to understand that they can, and often do, update. As such, it is important to monitor application behavior over time to ensure the data is still being provided in a format that is compatible with application logic. Further, care should be taken within the application to handle any changes are handled gracefully within the application so that they don't hinder the user experience.

**Server Errors** - Even when a remote data source is providing the proper information, there is always a chance that the remote source experiences errors or delays that prevent communication with your application altogether. As such, the application should gracefully handle a lack of response from the remote source to avoid interfering with the user experience.

**Network Outage** - As with Server Errors, mobile networks aren't always reliable. As such, the application should be aware of its network status at all times, and prevent the user for taking actions which require network access when none is available - such as when making an API call. In the event the network becomes unavailable during an API call, the application should, again, gracefully handle the errors to avoid interrupting the experience for the end user.