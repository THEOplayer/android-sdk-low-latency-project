# THEOplayer Android [TV] SDK with Low Latency Manager

## License

This projects falls under the license as defined in https://github.com/THEOplayer/license-and-disclaimer.

## Getting Started

This Android project is an example how to integrate [THEOplayer](https://www.theoplayer.com) into an Android [TV] app with Latency Manager Plugin by THEOplayer.

## How to implement a basic latency manager

This how-to-guide explains how to implement a basic latency manager for the THEOplayer sdk. This plugin is only valid for CMAF Low Latency DASH Streams. 

## Prerequisites

1. Setup a Android Project and include the THEOplayer library and Low latency DASH stream. You can also check: [How to get started with THEOplayer Android SDK](https://docs.portal.theoplayer.com/getting-started/01-sdks/02-android/00-getting-started.md)

2. You would need a Android SDK of THEOplayer with basic configuration to setup your environment. You can create an SDK by signing-in to the THEO Portal [Login Here](https://portal.theoplayer.com/login)

## Quick Start to run the demo Android [TV] with Latency Manager 

1. Obtain THEOplayer Android SDK and unzip it.

   Please visit [Get Started with THEOplayer] to get required THEOplayer Android SDK.

2. Copy **`theoplayer-android-[name]-[version]-minapi16-release.aar`** file from unzipped SDK into
   application **[libs]** folder and rename it to **`theoplayer.aar`**.

   Project is configured to load SDK with such name, for using other name please change
   `implementation ':theoplayer@aar'` dependency in [app-level build.gradle] file accordingly.

3. Open **Android with Latency Manager** application in Android Studio.

   Android Studio should automatically synchronize and rebuild project. If this won't happen please
   select **File > Sync Project with Gradle Files** menu item to do it manually. Please note, that
   in very rare cases it will be required to synchronize project twice.

4. Select **Run > Run 'app'** menu item to run application on a device selected by default.

   To change the device please select **Run > Select Device...** menu item. 

## Code Example to setup Latency manager in Android Project [TV]

1. Download the [latency-manager js file (zipped)](https://cdn.theoplayer.com/LatencyManager.zip). Extract the zipped file and save it in the assests folder of the Android project. 

2. Add the Custom JS code to the `theoplayerView`, You can also read the article [How to add CSS or JavaScript files to an Android/iOS project](https://docs.portal.theoplayer.com/faq/01-how-to-add-css-or-javascript-files-to-android-ios.md)

3. Add a new `LatencyManger`, `LatencyManagerConfiguration`, `LatencyManagerConfigurationBuilder` & `LatencyParameters` Java class and add repective code as like [demo project](https://github.com/THEOplayer/android-sdk-low-latency-project)

4. Add the below params of THEOplayer Source as well:

```java
        // Creating a TypedSource builder that defines the location of a single stream source
        TypedSource typedSource = TypedSource.Builder
                .typedSource()
                .src(getString(R.string.defaultSourceUrl))
                .liveOffset(1.0)
                .lowLatency(true)
                .timeServer("https://time.akamai.com/?iso&ms=true") //There is a Timeserver Offered by THEOplayer also https://time.theoplayer.com
                .type(SourceType.DASH)
                .build();


        // Creating a SourceDescription builder that contains the settings to be applied as a new THEOplayer source.
        SourceDescription.Builder sourceDescription = sourceDescription(typedSource);

        //Setting the source to the player
        player.setSource(sourceDescription.build());
```

4. Initialise the `latencymanager` with the following properties for the player. Example as below: 

```java
//Intialise the Latency Manager Configuration
        LatencyManagerConfiguration config = new LatencyManagerConfigurationBuilder()
                .targetLatency(2000)  //target latency value the player must acheive
                .timeServer("https://time.theoplayer.com") //instance of TimeServer must support timeserver.getServerTime() : Date()
                .interval(200) //frequency of the update event to be fired 200 is in ms
                .fireUpdate(true) //To keep sending the data between the Javascript and Java
                .latencyWindow(250) //window around targetlatency the manager will consider in sync
                .rateChange(0.08)  ////maximum increase/decrease in speed of the player
                .seekWindow(5000)  // //window around targetlatency the manager considers to fire seek command rather than change playbackrate
                .sync(true).build(); //Set to true to use the Latency Manager to sync with the configs 
        
        //Intialise the Latency Manager with the defined config
        latencyManager = new LatencyManager(viewBinding.theoPlayerView,config);
```
* Note: The default values of the Latency Manager params are as below:
```javascript
            this.targetlatency = 5000;
            this.timeserver = "https://time.theoplayer.com";
            this.interval = 200;
            this.fireupdate = true;
            this.latencywindow = 250;
            this.ratechange = 0.08;
            this.seekwindow = 5000;
            this.sync = true;
            
```

## Resources
- [Getting started with the Web SDK](https://docs.portal.theoplayer.com/getting-started/01-sdks/01-web/00-getting-started.md)
- [How to get started with THEOplayer Android SDK](https://docs.portal.theoplayer.com/getting-started/01-sdks/02-android/00-getting-started.md)
- [How to add CSS or JavaScript files to an Android/iOS project](https://docs.portal.theoplayer.com/faq/01-how-to-add-css-or-javascript-files-to-android-ios.md)
- [Low Latency Android demo project](https://github.com/THEOplayer/android-sdk-low-latency-project)