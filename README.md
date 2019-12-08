# WowzaVideoStreaming

The Wowza Streaming Cloud allows live-streaming video service on a global cloud platform and API, using the Wowza GoCoder SDK.
This is a simplified global streaming service, that allows the user to configure professional streams for an audience of anysize around the world. The streaming quality is auto-scaling to proactively adjust and solve issues, while providing high quality reliability and performance. With the use of the Wowza GoCoder SDK, the user can deploy a working live stream within minutes and allowing for a global platform that can deliver to audiences of any size.

In order for the Wowza Streaming Cloud to work, the user must download the Wowza GoCoder SDK. This SDK must be included in the project's files to be used as a library to reference.

Wowza has transformed over the years as it was being developed. It was initially a simple program that allowed a live, on-demand video and audio streaming using the Flash format. Over 2005-2014, the software improved to support several mulitple protocols and sources, and to be able to deliver to any device. It improved the functionality of live streaming by introduced transcoding, nDVR, and DRM, ensureing the best audio and visual experience for its time.

In 2014, the Wowza Streaming Engine was released, which allowed the streamed content to be more controlled and easily customizable through richer APIs and hardware infrastructures. Finally in 2015, the Wowza Streaming Cloud was introduced. This allowed an end-to-end streaming service that complemented the Wowza Streaming Engine. This resulted in a more flexible cloud streaming service in terms of development scale; it eliminated the need to build and deploy programs for the purpose of streaming, and it creates streaming accessible for any budget and any scale of audience.

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
3. In the layout file (activity_main.xml), the camera preview is set to fill the entire screen, and a button is created to be the "Broadcast" button
4. The AndroidManifest.xml is updated to reflect the activity orientation.
5. Enable the sticky full-screen mode using the onWindowFocusChanged() method.
6. Member variables from the Wowza GoCoder Library are added.
7. The GoCoder SDK is initialized using the registered license key. This will follow the onCreate method, and will need to use the license key that the user has received when registering for the SDK license.
8. The onResume() and onRequestPermissionResult() methods are used to ask the user for permissions. This will ask for the camera and microphone permissions. hasPermissions() method will check the status of permissions granted by the user.
9. Defines the view of the camera_preview
10. Activates the camera preview when the app is brought to the foreground.
11. Create an audio device instance for capturing and broadcasting audio
12. Defines a broadcast object using WOWZBroadcast() method, and its connection properties are also defined
    - The properties can be found when the user sets up a live stream, as mentioned in the prerequisites.
    - Several methods are used to set the connection properties:
        - setHostAddresss(): The IP address of the Wowza Streaming Engine Instance
        - setPortNumber(): Port number used for the server connection
        - setApplicationName(): Name of the livestreaming application, can be found on the primary server information
        - setStreamName(): The given stream name by the website
        - setUserName(): The given Source Username
        - setPassword(): The given Source Password
13. Broadcast monitoring callbacks are added, which is used to monitor the broadcast for status updates and errors. It uses a switch-case in order to handle different statuses of the broadcast.
14. The callback invoked when the broadcast button is tapped. This will enable streaming when the broadcast button is pressed, or stops streaming when there is already an ongoing broadcast.


## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
