#Organizing Your Project
In Android, your package name is the unique identifier for your application. However, Java packages can also be used to organize code into specialized groupings. In this section, we'll discuss how to create a unique package name for your app and how to use Java packages to organize your code.

##Package Names

As covered previously, Android Studio will ask you to enter in a package name for your application. This package name will be used as the identifier for your app and must be unique to your specific application. With millions of apps already available on Google Play, it might seem like a daunting task to come up with a unique package name. There is, however, an easy way to come up with this name - reverse domain notation. 

Reverse domain notation is where you take your personal or company website, and reverse the domain. So for "company.com", the reverse domain would be "com.company". Starting your package name this way ensures that your app comes from you or your company, which will greatly limit the number of people using this package name. Since all package names need to be unique, you can't have more than one app with just your domain. It's good practice to add two more things to your package name, platform and project name. Java is a cross-platform language and, as such, can be used across desktop, the web, mobile, and any other device that supports a Java virtual machine. If you're building a Java app for Android and the desktop, it's a good idea to distinguish your projects by adding the platform in the package name. Lastly, you should always add your project name to the end of your package name to help identify the specific project.

###Example
Let's say we're building a calculator app for Android and the desktop and the app is going to be released by Cool Development Company. The package name for this would be the reverse domain, the platform, and the app name. A good Android package identifier might be:
```
com.cooldev.android.calculator
```
While a good desktop identifier could be:
```
com.cooldev.desktop.calculator
```
All of the apps you build in this book will be for Android so you can choose whether or not to include the platform, but it's helpful to denote platform if you work on any cross-platform apps in the future.

##Organizing Code

