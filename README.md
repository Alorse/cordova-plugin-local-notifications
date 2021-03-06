Cordova LocalNotification-Plugin
==================================

A bunch of local notification plugins for Cordova 3.x.x

by Sebastián Katzer ([github.com/katzer](https://github.com/katzer))

## Supported Platforms
- **iOS**<br>
See [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/WhatAreRemoteNotif.html) for detailed informations and screenshots.

- **Android**<br>
See [Notification Guide](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) for detailed informations and screenshots.

- **WP8**<br>
See [Local notifications for Windows Phone](http://msdn.microsoft.com/en-us/library/windowsphone/develop/jj207047.aspx) for detailed informations and screenshots.
<br>*Windows Phone 8.0 has no notification center. Instead local notifications are realized through live tiles updates.*


## Dependencies
Cordova will check all dependencies and install them if they are missing.
- [org.apache.cordova.device](https://github.com/apache/cordova-plugin-device) *(since v0.6.0)*


## Adding the Plugin to your project
Through the [Command-line Interface](http://cordova.apache.org/docs/en/3.0.0/guide_cli_index.md.html#The%20Command-line%20Interface):
```
cordova plugin add https://github.com/katzer/cordova-plugin-local-notifications.git
```

## Removing the Plugin from your project
Through the [Command-line Interface](http://cordova.apache.org/docs/en/3.0.0/guide_cli_index.md.html#The%20Command-line%20Interface):
```
cordova plugin rm de.appplant.cordova.plugin.local-notifications
```

## Release Notes
#### Version 0.6.1 (not yet released)
- [feature:] Sound can be specified under Android.
- [enhancement:] Adding notifications on Android does not block the ui thread anymore.

#### Version 0.6.0 (16.11.2013)
- Added WP8 support<br>
  *Based on the LiveTiles WP8 plugin made by* ***Jesse MacFadyen (purplecabbage)***
- [enhancement:] The `add()` function now returns the id of the created notification.
- [feature:] Added new `title` property.
- [bugfix:] `cancel` under iOS did not work do to wrong dict key.
- [enhancement:] All notifications under Android display the app icon by default.
- [feature:] Icon can be specified under Android.

#### Version 0.4.0 (06.10.2013)
- Added Android support<br>
  *Based on the LocalNotifications Android plugin made by* ***Daniël (dvtoever)***

#### Version 0.2.0 (11.08.2013)
- Added iOS support<br>
  *Based on the LocalNotifications iOS plugin made by* ***Rodrigo Moyle***


## Using the plugin
The plugin creates the object ```window.plugin.notification.local``` with the following methods:

### add()
The method allows to add a custom notification. It takes an hash as an argument to specify the notification's properties and returns the ID for the notification.<br>
All properties are optional. If no date object is given, the notification will pop-up immediately.

```javascript
window.plugin.notification.local.add({
    id:         String, // a unique id of the notifiction
    date:       Date,   // this expects a date object
    message:    String, // the message that is displayed
    title:      String, // the title of the message
    repeat:     String, // has the options of daily', 'weekly',''monthly','yearly')
    badge:      Number, // displays number badge to notification
    sound:      String, // a sound to be played (iOS & Android)
    foreground: String, // a javascript function to be called if the app is running
    background: String, // a javascript function to be called if the app is in the background
});
```

### cancel()
The method cancels a notification which was previously added. It takes the ID of the notification as an argument.
```javascript
window.plugin.notification.local.cancel(__id__);
```

### cancelAll()
The method cancels all notifications which were previously added by the application.
```javascript
window.plugin.notification.local.cancelAll();
```


## Example
```javascript
var now                  = new Date().getTime(),
    _60_seconds_from_now = new Date(now + 60*1000);

window.plugin.notification.local.add({
    id:         1,                       // is converted to a string
    date:       _60_seconds_from_now,
    message:    'Hello world!',
    title:      'Check that out!',
    repeat:     'weekly',                // will fire every week on this day
    foreground: 'foreground',
    background: 'background'
});

function foreground (id) {
    console.log('I WAS RUNNING ID='+id)
}

function background (id) {
    console.log('I WAS IN THE BACKGROUND ID='+id)
}
```


## Platform specifics
### Notification icon under Android
By default all notifications will display the app icon. But an specific icon can be defined through the `icon` property.
```javascript
/**
 * Displays the <package.name>.R.drawable.ic_launcher icon
 */
window.plugin.notification.local.add({ icon: 'ic_launcher' });

/**
 * Displays the android.R.drawable.ic_dialog_email icon
 */
window.plugin.notification.local.add({ icon: 'ic_dialog_email' });
```

### Notification sound under Android
The default sound is `RingtoneManager.TYPE_NOTIFICATION`. But an specific sound can be defined through the `sound` property.<br>
The sound must be a absolute or relative Uri pointing to the sound file.
```javascript
/**
 * Plays the sound if the notification pop's up.
 */
window.plugin.notification.local.add({ sound: 'res/sounds/beep.mp3' });

/**
 * Plays the `RingtoneManager.TYPE_ALARM` sound.
 */
window.plugin.notification.local.add({ sound: 'TYPE_ALARM' });

/**
 * Plays no sound if the notification pop's up.
 */
window.plugin.notification.local.add({ sound: null });
```

### Notification sound under iOS
The sound must be located in your project's resources and must be a caf file.
```javascript
/**
 * Plays the sound if the notification pop's up.
 */
window.plugin.notification.local.add({ sound: 'sub.caf' });
```
**Note:** The right to play notification sounds in the notification center settings has to be granted.

### LiveTile background images under WP8
LiveTile's have the ability to display images for different sizes. These images can be defined through the `smallImage`, `image` and `wideImage` properties.<br>
An image must be defined as a relative or absolute URI.
```javascript
/**
 * Displays the application icon as the livetile's background image
 */
window.plugin.notification.local.add({ image: 'appdata:ApplicationIcon.png' })
```
All images can be restored to the default ones by canceling the notification.


## Quirks
### No sound is played on iOS 7
The right to play notification sounds in the notification center settings has to be granted.
### Adding a notification under WP8
An application can only display one notification at a time. Each time a new notification has to be added, the application live tile's data will be overwritten by the new ones.

### Canceling a notification under WP8
The methods `cancel` and `cancelAll` have the same effect.


## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
