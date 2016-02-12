# Web Based Applications

The majority of this book is about native Android development, so why switch to web apps now?


### Quick "Native" Apps

There's a reputation in mobile that if you don't have an app, you're not prepared for the mobile space. Using a WebView and the native activity stack for navigation, web apps enable mobile sites to have a quick mobile app in the store.  This practice isn't allowed in iOS, but Google Play still allows it for published applications.

### JavaScript Libraries

Android is great.  The SDK contains components for almost everything you'd want to do in an app and there are a great deal of libraries to fill in the gaps.  Still, there are some things Android and Java don't do well.  Take a look at the following example:

| ![GraphView](GraphView.png) | ![d3js](d3js.png) |
| :--: | :--: |
| [GraphView](http://www.android-graphview.org/) | [d3js](https://d3js.org/) |

On the left is the open source GraphView library. While the library provides a native component that you can drop into your layout, it's limited in features.  On the right side is the open source D3 JavaScript library. D3 takes a bit longer to setup since it's in JavaScript and requires a WebView, but it provides far more functionality and customization than any available Java or Android library.

## Building Web Apps

Showing web content or local HTML/JS files is accomplished using the WebView class. This is a regular old view that you can drop into your layout in much the same way as you would a Button or TextView.  On its own, the WebView can show local HTML and JavaScript content. By adding the INTERNET permission to the manifest, a WebView can show web based content without any extra code.

In order to show HTML and JavaScript files locally, the files need to be added into the project. However, adding these files to the resource directory will cause them to get crunched down during compilation and not show properly. Instead, these files need to be located in the assets folder.  This folder isn't created for you by default but can be added by right clicking on your app module in Android Studio and selecting New -> Folder -> Assets Folder.

**Warning About the Assets Folder:** Any application resources added to the assets folder are at risk of being pirated by savvy user.  When building an APK, the Android compiler does not touch the files in the assets folder and they're included in the APK in their original form.  Since APK files are just ZIP files with a different extension, any user could grab your APK, change the extension, unzip it, and see everything in the assets folder.  Because of this, you should never include any sensitive data in files in the assets folder.

Consider the following example code:

```
// Grab the WebView like any other view.
WebView wv = (WebView)findViewById(R.id.webview);
    
// JavaScript must be enabled in order to execute.
WebSettings ws = wv.getSettings();
ws.setJavaScriptEnabled(true);
    
// Load a URL in the WebView.
wv.loadUrl("http://www.google.com");
    
// Load a file from the assets folder.
wv.loadUrl("file:///android_asset/index.html");
```
    
Assuming all other code is correct and our permissions are setup properly, if you run this code in an app, the WebView doesn't load any data. Instead, the native browser app on the device will open and load the given URL.  This happens because the WebView doesn't know how to handle loading links on its own. It needs a web client in order to handle this functionality.

### WebViewClient