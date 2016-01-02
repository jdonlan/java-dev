# Audio Playback

In addition to the traditional music player and streaming music apps, it's becoming more and more commonplace to see artists or musicians have their own branded apps that play their own music. It's also very common for non-music oriented apps to play a sound effect here or there to enhance the feedback of certain actions. To play music or other audio effects on Android, we'll be using the MediaPlayer class. We could also play video with the MediaPlayer class, but there's a lot more setup involved with using a MediaPlayer instead of a VideoView. Because of this, we'll use VideoView for simple video playback and MediaPlayer for audio playback.

##MediaPlayer States

One of the key differences between playing video and playing audio is the need to maintain state. The MediaPlayer class has several different states in which it can exist and each state requires certain methods to be called to transition from one state to the next. Additionally, states have a limited number of other states that they can transition to, so you need to keep track of what methods have been called and what state your player is in at all times. For a full state diagram that shows which state the MediaPlayer starts in and what methods to call to transition between states, take a look at the MediaPlayer documentation provided in the resources section of this section. The main states you need to know are listed below.

* Idle - MediaPlayer created, no methods called. Must be initialized in order to use.
* Initialized - Data source is set to the media player. Must be prepared in order to play audio.
Prepared - MediaPlayer has been prepared synchronously or asynchronously and can now play audio. Audio cannot be played until the player has been prepared.
* Started - Audio is playing. Audio can be paused or stopped at this point.
* Paused - Audio is paused. Audio can be resumed (started) or stopped at this point.
* Stopped - Audio is fully stopped. Must prepare the player again before play can resume.
* End - MediaPlayer has been released using the release() method. Player must be recreated in order to use again.

Keep the states in mind as they'll be referenced quite a bit as we go through the code. To start this example, we'll need to create a new MediaPlayer object. Once created, the player starts out in the idle state and must be initialized before it can be used. To initialize the player, we'll set a data source using a URI that points to a raw audio resource. You can alternatively set the URI to point to a web resource, but be sure to include the INTERNET permission in your manifest for that to work.

```
// Player created, in idle state.
MediaPlayer player = new MediaPlayer();
 
// Player initialized, in initialized state.
player.setDataSource(this, Uri.parse("android.resource://..."));
```

Now that the player has been created and initialized and it's sitting in the initialized state, it must be prepared in order for it to play any sort of audio. There are two methods to prepare audio, each with their own uses. The first method is just called prepare(). Calling this method will prepare the player and return when the player is in the prepared state. When using small resources for your data source, this returns very quickly. However, a call to prepare() can take a long time for large resources or resources hosted on the internet. Because this has the potential for being a long running operation, we shouldn't call this from the main thread.

Instead of calling prepare(), there is another way to prepare the player; a call to prepareAsync(). Calling prepareAsync() will force the player to do all of its preparing operations in a background thread without you having to set all of that up yourself. Since the player is preparing in the background, we need a way to know when the player is done preparing. Before calling prepareAsync(), you'll want to create a new OnPreparedListener and set the listener to the player using the setOnPreparedListener() method. Then, when the player is done preparing, you can continue on to playing your audio.

```
// Player created, in idle state.
MediaPlayer player = new MediaPlayer();
 
// Player initialized, in initialized state.
player.setDataSource(this, Uri.parse("android.resource://..."));
// Setting a prepared listener.
player.setOnPreparedListener(new OnPreparedListener() {
	@Override
	public void onPrepared(MediaPlayer mp) {
		// Player prepared!
		mp.start();
	}
});
```

That's it for getting media playing. However, if this is being done in an activity, then you'll need to make sure you're managing these states in line with the activity life-cycle methods. If your activity is paused, make sure your music is paused. If your activity resumes, check if the music was playing and resume if necessary. When your activity stops, stop the player. Then, when your activity is destroyed, release your player to clean up any lingering resources. Keep in mind that if your player is stopped, you'll have to prepare the player again before you can play music. Once prepared, the MediaPlayer will stay in the prepared state until the player is either stopped or released, unless an error is thrown at anytime during playback at which point stop() is called for you.

With your media playing in the activity, you have a pretty major limitation. When your activity stops, the music stops with it. However, there will be times where you'll want your music to continue playing even when the app is closed. To do this, you'll need to handle your audio playing in a service. Services have very similar life-cycle callbacks as activities do, with some differences in naming. When moving your audio playing to a service, keep in mind which methods translate into which states, and which states can transition to other states. Then, you can just map the MediaPlayer states to the service life-cycle callbacks.

###Resources
[Android Developers: Media Playback (helpful)](http://developer.android.com/guide/topics/media/mediaplayer.html)<br/>
Android developer guide that walks through the process of playing audio using the MediaPlayer class. Covers topics such as player state, background audio, and handling audio focus changes.

[Android Developers: MediaPlayer (helpful)](http://developer.android.com/reference/android/media/MediaPlayer.html)<br/>
Android developer documentation for the MediaPlayer class. Covers the different player states, which states can transition to other states, and how to handle errors in playback. This documentation also serves as a helpful reference when looking up MediaPlayer methods and functionality.

[Android Developers: Managing Audio Focus (helpful)](http://developer.android.com/training/managing-audio/audio-focus.html)<br/>
Your app will likely not be the only app capable of playing audio on the device. In case other apps need to play audio, you need to make sure that your app properly handles pausing or ducking depending on the audio focus state. This is really only required if you're making a music player so it's included here as an additional resource.