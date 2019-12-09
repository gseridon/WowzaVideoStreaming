# WowzaVideoStreaming

The Wowza Streaming Cloud allows live-streaming video service on a global cloud platform and API, using the Wowza GoCoder SDK.
This is a simplified global streaming service, that allows the user to configure professional streams for an audience of anysize around the world. The streaming quality is auto-scaling to proactively adjust and solve issues, while providing high quality reliability and performance. With the use of the Wowza GoCoder SDK, the user can deploy a working live stream within minutes and allowing for a global platform that can deliver to audiences of any size.

In order for the Wowza Streaming Cloud to work, the user must download the Wowza GoCoder SDK. This SDK must be included in the project's files to be used as a library to reference.

### Brief History

Wowza has transformed over the years as it was being developed. It was initially a simple program that allowed a live, on-demand video and audio streaming using the Flash format. Over 2005-2014, the software improved to support several mulitple protocols and sources, and to be able to deliver to any device. It improved the functionality of live streaming by introduced transcoding, nDVR, and DRM, ensureing the best audio and visual experience for its time.

In 2014, the Wowza Streaming Engine was released, which allowed the streamed content to be more controlled and easily customizable through richer APIs and hardware infrastructures. With the latest addition of the Wowza Streaming Engine Pro, not only allows support for MPEG-DASH, but it also incorporates the Wowza Transcoder with nDVR, DMR, and unlimited channels.

Finally in 2015, the Wowza Streaming Cloud was introduced. This allowed an end-to-end streaming service that complemented the Wowza Streaming Engine. This resulted in a more flexible cloud streaming service in terms of development scale; it eliminated the need to build and deploy programs for the purpose of streaming, and it creates streaming accessible for any budget and any scale of audience.

### Prerequisites

Before getting started, there are a couple of programs that need to be installed:

- <a href="https://developer.android.com/studio">Android Studio</a> (Version 3.4.0 or later)
- <a href="https://www.wowza.com/pricing/installer#gocodersdk-downloads">Wowza GoCoder SDK</a>

In order to download the SDK, the user must sign up for a <a href="https://www.wowza.com/pricing/cloud-developer-free-trial">Wowza Streaming Free Trial</a>. This will allow the user to get their development license code, which will be used in the program code to enable the streaming platform.

The user should also set up a live stream beforehand, so that they will be able to input connection data into the code that will be needed later on. To set up a live stream:

- Go to <a href="www.cloud.wowza.com">Wowza Streaming Cloud</a> and sign in your account
- Click on livestreams on the menu bar and add a live stream
- Provide a name for the livestream and select a location that is closest to where the user will be broadcasting from
- Click next until the set-up is finished (these configurations will be default and can be changed later on
- The newly created livestream page will contain information needed to connect the app to the live stream


### The Code

The build instructions for the program can be found <a href="https://www.wowza.com/docs/how-to-build-a-basic-app-with-gocoder-sdk-for-android">here</a>.

The Wowza GoCorder SDK requires Android 5.0 (API 21) or later to enable broadcasting, while video playback requires Android 6.0 (API 23). This project was built under API 21, with Java used as the coding language. Load the project in Android studios.

It is important to have the Wowza GoCoder SDK downloaded in the PC. Inside the folder, there will be a file called "com.wowza.gocoder.sdk.aar". This file must be copied into the project's app/libs folder to make it easier to reference the SDK.

The following code is already implemented in the project with associated comments.
These points are found numerically in the code as the TODO list:

1. The SDK must be referenced in the build.gradle(Module: app) file using:

```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```
```
dependencies {
    implementation 'com.wowza.gocoder.sdk.android:com.wowza.gocoder.sdk:2.0.0@aar'
    }
```
2. Permissions must be added in the AndroidManifest.xml file in order to access some of the features (such as the camera).
```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
```

3. In the layout file (activity_main.xml), the camera preview is set to fill the entire screen, and a button is created to be the "Broadcast" button. This layout uses a WOWZCameraView to hold the camera preview, and uses a button for Broadcast.

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  xmlns:tools="http://schemas.android.com/tools"
  xmlns:wowza="http://schemas.android.com/apk/res-auto"
  android:id="@+id/activity_main"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  tools:context=".MainActivity">

  <!-- The camera preview display -->
  <com.wowza.gocoder.sdk.api.devices.WOWZCameraView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/camera_preview"
    wowza:scaleMode="fill"
    wowza:defaultCamera="back"
    wowza:frameSizePreset="frameSize1280x720"/>

  <!-- The broadcast button -->
  <Button
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Broadcast"
    android:id="@+id/broadcast_button"
    android:layout_alignParentBottom="true"
    android:layout_centerHorizontal="true" />

</RelativeLayout>
```

4. The AndroidManifest.xml is updated to reflect the activity orientation.

```
<activity android:name=".MainActivity"
    android:configChanges="orientation|keyboardHidden|screenSize">
```

5. Enable the sticky full-screen mode using the onWindowFocusChanged() method to detect if the app has focus.

```
//
// Enable Android's immersive, sticky full-screen mode
//
@Override
public void onWindowFocusChanged(boolean hasFocus) {
  super.onWindowFocusChanged(hasFocus);

  View rootView = getWindow().getDecorView().findViewById(android.R.id.content);
  if (rootView != null)
    rootView.setSystemUiVisibility(
        View.SYSTEM_UI_FLAG_LAYOUT_STABLE
            | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
            | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
            | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
            | View.SYSTEM_UI_FLAG_FULLSCREEN
            | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY);
}
```

6. Member variables from the Wowza GoCoder Library are added. Major classes include:
- WowzaGoCoder: initializes the GoCoder SDK and to get access to device and platform information
- WOWZCameraView: a custom class for previewing the video captured from a camera
- WOWZAudioDevice:  a class used to encode audio streams
- WOWZBroadcast: a class used to configure and control live streaming broadcasts
- WOWZBroadcastConfig: a class that provides configuration properties for a live streaming broadcast

```
public class MainActivity extends AppCompatActivity {

    // The top-level GoCoder API interface
    private WowzaGoCoder goCoder;

    // The GoCoder SDK camera view
    private WOWZCameraView goCoderCameraView;

    // The GoCoder SDK audio device
    private WOWZAudioDevice goCoderAudioDevice;

    // The GoCoder SDK broadcaster
    private WOWZBroadcast goCoderBroadcaster;

    // The broadcast configuration settings
    private WOWZBroadcastConfig goCoderBroadcastConfig;

    // Properties needed for Android 6+ permissions handling
    private static final int PERMISSIONS_REQUEST_CODE = 0x1;
    private boolean mPermissionsGranted = true;
    private String[] mRequiredPermissions = new String[] {
            Manifest.permission.CAMERA,
            Manifest.permission.RECORD_AUDIO
    };
```

7. The GoCoder SDK is initialized using the registered license key. This will follow the onCreate method, and will need to use the license key that the user has received when registering for the SDK license.

```
// Initialize the GoCoder SDK
goCoder = WowzaGoCoder.init(getApplicationContext(), "GOSK-XXXX-XXXX-XXXX-XXXX-XXXX");

if (goCoder == null) {
    // If initialization failed, retrieve the last error and display it
    WOWZError goCoderInitError = WowzaGoCoder.getLastError();
    Toast.makeText(this,
        "GoCoder SDK error: " + goCoderInitError.getErrorDescription(),
        Toast.LENGTH_LONG).show();
    return;
}
```

8. The onResume() and onRequestPermissionResult() methods are used to ask the user for permissions. This will ask for the camera and microphone permissions. The hasPermissions() method will check the status of permissions granted by the user.

```
//
// Called when an activity is brought to the foreground
//
@Override
protected void onResume() {
    super.onResume();

    // If running on Android 6 (Marshmallow) and later, check to see if the necessary permissions
    // have been granted
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
        mPermissionsGranted = hasPermissions(this, mRequiredPermissions);
        if (!mPermissionsGranted)
            ActivityCompat.requestPermissions(this, mRequiredPermissions, PERMISSIONS_REQUEST_CODE);
    } else
        mPermissionsGranted = true;

}

//
// Callback invoked in response to a call to ActivityCompat.requestPermissions() to interpret
// the results of the permissions request
//
@Override
public void onRequestPermissionsResult(int requestCode, String permissions[], int[] grantResults) {
    mPermissionsGranted = true;
    switch (requestCode) {
        case PERMISSIONS_REQUEST_CODE: {
            // Check the result of each permission granted
            for(int grantResult : grantResults) {
                if (grantResult != PackageManager.PERMISSION_GRANTED) {
                    mPermissionsGranted = false;
                }
            }
        }
    }
}

//
// Utility method to check the status of a permissions request for an array of permission identifiers
//
private static boolean hasPermissions(Context context, String[] permissions) {
    for(String permission : permissions)
        if (context.checkCallingOrSelfPermission(permission) != PackageManager.PERMISSION_GRANTED)
            return false;

    return true;
}
```

9. Defines the view of the camera_preview with the corresponding WOWZCameraView class. This is added after the onCreate() method.

```
// Associate the WOWZCameraView defined in the U/I layout with the corresponding class member
goCoderCameraView = (WOWZCameraView) findViewById(R.id.camera_preview);
```

10. Activates the camera preview when the app is brought to the foreground.

```
// Start the camera preview display
if (mPermissionsGranted && goCoderCameraView != null) {
    if (goCoderCameraView.isPreviewPaused())
        goCoderCameraView.onResume();
    else
        goCoderCameraView.startPreview();    
}
```

11. Your app needs to deliver the live stream data to the Wowza Streaming Cloud, which is why we need to create a WOWZBroadcast object instance. Firstly, create an audio device instance for capturing and broadcasting audio. 

```
// Create an audio device instance for capturing and broadcasting audio
goCoderAudioDevice = new WOWZAudioDevice();
```

12. Defines a broadcast object using WOWZBroadcast() method, and its connection properties are also defined
    - The properties can be found when the user sets up a live stream, as mentioned in the prerequisites.
    - Several methods are used to set the connection properties:
        - setHostAddresss(): The IP address of the Wowza Streaming Engine Instance
        - setPortNumber(): Port number used for the server connection
        - setApplicationName(): Name of the livestreaming application, can be found on the primary server information
        - setStreamName(): The given stream name by the website
        - setUserName(): The given Source Username
        - setPassword(): The given Source Password
        
```
// Create a broadcaster instance
goCoderBroadcaster = new WOWZBroadcast();

// Create a configuration instance for the broadcaster
goCoderBroadcastConfig = new WOWZBroadcastConfig(WOWZMediaConfig.FRAME_SIZE_1920x1080);

// Set the connection properties for the target Wowza Streaming Engine server or Wowza Streaming Cloud live stream
goCoderBroadcastConfig.setHostAddress("live.someserver.net");
goCoderBroadcastConfig.setPortNumber(1935);
goCoderBroadcastConfig.setApplicationName("live");
goCoderBroadcastConfig.setStreamName("myStream");

// Designate the camera preview as the video source
goCoderBroadcastConfig.setVideoBroadcaster(goCoderCameraView);

// Designate the audio device as the audio broadcaster
goCoderBroadcastConfig.setAudioBroadcaster(goCoderAudioDevice);
```

13. Broadcast monitoring callbacks are added, which is used to monitor the broadcast for status updates and errors. It uses a switch-case in order to handle different statuses of the broadcast.

```
//
// The callback invoked upon changes to the state of the broadcast
//
@Override
public void onWZStatus(final WOWZBroadcastStatus goCoderStatus) {
 // A successful status transition has been reported by the GoCoder SDK
 final StringBuffer statusMessage = new StringBuffer("Broadcast status: ");

 switch (goCoderStatus.getState()) {
  case BroadcastState.READY:
   statusMessage.append("Ready to begin broadcasting");
   break;

  case BroadcastState.BROADCASTING:
   statusMessage.append("Broadcast is active");
   break;

  case BroadcastState.IDLE:
   statusMessage.append("The broadcast is stopped");
   break;

  default:
   return;
 }

 // Display the status message using the U/I thread
 new Handler(Looper.getMainLooper()).post(new Runnable() {
  @Override
  public void run() {
   Toast.makeText(MainActivity.this, statusMessage, Toast.LENGTH_LONG).show();
  }
 });
}

//
// The callback invoked when an error occurs during a broadcast
//
@Override
public void onWZError(final WOWZBroadcastStatus goCoderStatus) {
 // If an error is reported by the GoCoder SDK, display a message
 // containing the error details using the U/I thread
 new Handler(Looper.getMainLooper()).post(new Runnable() {
  @Override
  public void run() {
   Toast.makeText(MainActivity.this,
   "Streaming error: " + goCoderStatus.getLastError().getErrorDescription(),
   Toast.LENGTH_LONG).show();
  }
 });
}
```

14. The callback invoked when the broadcast button is tapped. This button will trigger events depending on the status of the stream. This will enable streaming when the broadcast button is pressed, or stops streaming when there is already an ongoing broadcast.

```
//
// The callback invoked when the broadcast button is tapped
//
@Override
public void onClick(View view) {
  // return if the user hasn't granted the app the necessary permissions
  if (!mPermissionsGranted) return;

  // Ensure the minimum set of configuration settings have been specified necessary to
  // initiate a broadcast streaming session
  WOWZStreamingError configValidationError = goCoderBroadcastConfig.validateForBroadcast();

  if (configValidationError != null) {
    Toast.makeText(this, configValidationError.getErrorDescription(), Toast.LENGTH_LONG).show();
  } else if (goCoderBroadcaster.getStatus().isBroadcasting()) {
    // Stop the broadcast that is currently broadcasting
    goCoderBroadcaster.endBroadcast(this);
  } else {
    // Start streaming
    goCoderBroadcaster.startBroadcast(goCoderBroadcastConfig, this);
  }
}
```


## Running Tests and Deployment 

On the Wowza Streaming Cloud page, start the live stream. The user will be able to monitor the stream using the statistics on the side, or using the Stream Health Tab. When the stream is up and running, run the program on Android Studio. Allow the device to access the camera and microphone, then click the "Broadcast" button. This will broadcast the camera view online.

The broadcast preview should look like this, with the camera view updating every few seconds:

<img scr="https://raw.githubusercontent.com/gseridon/WowzaVideoStreaming/master/Images/AssignmentOutputStream.png">

Checking the Stream Health Tab will show that the program is sending data continuously to the stream for the broadcast to occur:

<img scr="https://raw.githubusercontent.com/gseridon/WowzaVideoStreaming/master/Images/AssignmentOutputStreamHealth.PNG">


## References

Build a live streaming app with GoCoder SDK for Android. (2019, October 23). Retrieved from https://www.wowza.com/docs/how-to-build-a-basic-app-with-gocoder-sdk-for-android.

GoCoder SDK for Android API Reference. (n.d.). Retrieved from https://www.wowza.com/resources/gocodersdk/docs/api-reference-android/.

How to Build a Live Streaming Broadcast App for Android. (2019, May 30). https://www.youtube.com/watch?v=hSq94hvlF6Q&t=
