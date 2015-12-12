# Device Push Notification Plugin

## DESCRIPTION

This plugin is for use with [Cordova](http://incubator.apache.org/cordova/), and allows your application to receive push notifications on Android and iOS devices.
* The Android implementation uses [Google's GCM (Google Cloud Messaging) service](http://developer.android.com/guide/google/gcm/index.html).
* The iOS version is based on [Apple APNS Notifications](http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html).

**Important** - Push notifications are intended for real devices. The registration process will fail on the iOS simulator. Notifications can be made to work on the Android Emulator, however doing so requires installation of some helper libraries, as outlined [here,](http://www.androidhive.info/2012/10/android-push-notifications-using-google-cloud-messaging-gcm-php-and-mysql/) under the section titled "Installing helper libraries and setting up the Emulator".

### Contents

- [Automatic Installation](#automatic_installation)
- [Plugin API](#plugin_api)
- [Testing](#testing)
- [LICENSE](#license)

##<a name="automatic_installation"></a>Automatic Installation

Below are the methods for installing this plugin automatically using command line tools. For additional info, take a look at the [Plugman Documentation](https://github.com/apache/cordova-plugman/blob/master/README.md) and [Cordova Plugin Specification](https://github.com/alunny/cordova-plugin-spec).

This requires phonegap/cordova 5.0+

### Supported Platforms

- Android
- iOS

### Platforms Under Development

- WP8

### Cordova and PhoneGap CLI

The plugin can be installed via the Cordova command line interface:

1) Navigate to the root folder for your phonegap project. 

2) Run the command:

```sh
cordova plugin add com.devicepush.cordova-phonegap
```
or
```sh
phonegap plugin add com.devicepush.cordova-phonegap
```

### PhoneGap Build Support

The plugin can be installed via PhoneGap Build:

1) Open config.xml file of your project. 

2) Add this line:

```xml
<gap:plugin name="com.devicepush.cordova-phonegap" source="npm" />
```

If you want to specify a particular version of the plugin you can add the version attribute to the gap tag.

```xml
<gap:plugin name="com.devicepush.cordova-phonegap" source="npm" version="0.3.8" />
```

##<a name="plugin_api"></a> Plugin API

#### Whitelist
Add *.devicepush.com domain in the config.xml file:
```xml
<access origin="*.devicepush.com" />
<allow-navigation href="*.devicepush.com" />
```

#### To register a new device
When the device is ready, you must call the register function.
```js
    var app = {
        initialize: function() {
            this.bindEvents();
        },
        bindEvents: function() {
            document.addEventListener('deviceready', this.onDeviceReady, false);
        },
        onDeviceReady: function() {
            app.receivedEvent('deviceready');
            devicePush.register({
                idUser: 'USER_ID', // Your User ID in Device Push
                idApplication: 'APPLICATION_ID', // Aplication ID in Device Push
                position: true // Activate or deactivate gps position record. Default value is false
                additionalData: {} // Currently in development
            });
        },
        receivedEvent: function(id) {}
    };
```

#### To get id or token device
You can get the device id or token of the device.
```js
    document.addEventListener("deviceRegistered", successDeviceRegistered, false);

    function successDeviceRegistered(evt){
        console.log("Device Id" + evt.devicePushId);
        var id = evt.devicePushId;
        console.log("Device Token" + evt.devicePushToken);
        var tokenDevice = evt.devicePushToken;
    }
```
With this ID you can send notification from your server.

#### To manager a notification received
You can manage notifications received with the next method
```js
    document.addEventListener('notificationReceived', successNotificationReceived, false);

    function successNotificationReceived(evt){
        // evt.data.message, 
        // evt.data.title, 
        // evt.data.count, 
        // evt.data.sound, 
        // evt.data.additionalData
        console.log(evt.data.message) // data is your text sent
    }
```

To show a dynamic and floating notification, you have to add the following function into the function successNotificationReceived.
```js
    devicePush.showNotification(evt.data.message);
```

#### To activate or not gps position record
You can activate or deactivate gps position record.
```js
    devicePush.setPosition(true); //Active gps position record, true o false. Default value is false.
```

#### To put additional user data for segmentation
To activate the segmentation of notifications, you will have to send additional user data, such as personal data.
```js
    // Currently in development
    devicePush.putAdditionalData({
        additionalData: {
            name: '',
            surnames: '',
            age: '',
            gender: ''
        } 
    });
```

You can see more information about this at: http://www.devicepush.com/documentation-push-notification/

Looking at the above message handling code for Android, a few things bear explanation. Your app may receive a notification while it is active (INLINE). If you background the app by hitting the Home button on your device, you may later receive a status bar notification. Selecting that notification from the status will bring your app to the front and allow you to process the notification (BACKGROUND). Finally, should you completely exit the app by hitting the back button from the home page, you may still receive a notification. Touching that notification in the notification tray will relaunch your app and allow you to process the notification (COLDSTART). In this case the **coldstart** flag will be set on the incoming event. You can look at the **foreground** flag on the event to determine whether you are processing a background or an in-line notification. You may choose, for example to play a sound or show a dialog only for inline or coldstart notifications since the user has already been alerted via the status bar.

Since the Android notification data models are much more flexible than that of iOS, there may be additional elements beyond **message**. You can access those elements and any additional ones via the **payload** element. This means that if your data model should change in the future, there will be no need to change and recompile the plugin.


##<a name="testing"></a> Testing
The notification system consists of several interdependent components.

##<a name="license"></a> LICENSE

	The MIT License

	Copyright (c) 2015 Device Push.
	portions Copyright (c) 2015 Device Push

	Permission is hereby granted, free of charge, to any person obtaining a copy
	of this software and associated documentation files (the "Software"), to deal
	in the Software without restriction, including without limitation the rights
	to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
	copies of the Software, and to permit persons to whom the Software is
	furnished to do so, subject to the following conditions:

	The above copyright notice and this permission notice shall be included in
	all copies or substantial portions of the Software.

	THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
	IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
	FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
	AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
	LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
	OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
	THE SOFTWARE.