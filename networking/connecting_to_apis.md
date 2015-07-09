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

