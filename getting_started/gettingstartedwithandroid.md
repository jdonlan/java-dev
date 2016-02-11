#Getting Started with Android

Android development is still young, but recent announcements by Google have started to solidify the standard development process and general workflow for Android development.  It is highly recommended that you use Android Studio as your IDE for Android development.

##Android Studio
Android Studio is a custom IDE originally built off the open source IntelliJ platform. Google has taken the platform and customized it for Android Development and is currently dedicating its efforts into the Android Studio environment as the development platform of choice for Android.

Unlike other IDE's that Google has supported with the Android Developer Tools plugin system, Android Studio is a fully Google controlled environment specifically customized and maintained for Android development.

###Android SDK and JDK7
Before you can begin programming for the Android platform, you will need to download the Android SDK and JDK7.  

###Project Organization
One of the most under or improperly-utilized features of development environments is workspaces. Workspaces allow the developer to organize and track project code effectively and efficiently, protecting project integrity and avoiding potentially significant errors due to conflicts that arise when working with multiple IDEs. In mobile development specifically, it isn't uncommon for a project to need multiple code bases to support multiple platforms for release. It is recommended that, at minimum, a workspace for each IDE is maintained. That said, it is also important not to overdo it and create too many workspaces. A workspace is NOT a project, and shouldn't be used as such. Instead, developers should use a workspace for as many projects as makes sense. 

A developer may have a workspace, for example, dedicated to Android (mobile) projects and another dedicated to Android Wear or Android TV projects. Another usage for workspaces would be for a developer to separate side projects and those they are working on for their employer. One thing is certain - each project should have its own project folder in an appropriate workspace.

####References
[https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html)