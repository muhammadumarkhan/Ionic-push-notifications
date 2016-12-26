## Installation

Install Phonegap push plugin through command line

cordova plugin add phonegap-plugin-push --variable SENDER_ID="XXXXXXX"

Where the XXXXXXX in SENDER_ID="XXXXXXX" maps to the project number in the Google Developer Console. To find the project number login to the Google Developer Console, select your project and click the menu item in the screen shot below to display your project number.

## Ionic code

```
$ionicPlatform.ready(function() {
  if (window.PushNotification) {
    var push = window.PushNotification.init({
      "android": {
        "senderID": "xxxxxxxxxxx"
      },
      "ios": {"alert": "true", "badge": "true", "sound": "true"}, 
      "windows": {} 
    });
    
    push.on('registration', function(data) {
      $rootScope.device_token = data.registrationId;
      console.log("registration event", data.registrationId);
    });

    push.on('notification', function(data) {
      if (data.additionalData.path && data.additionalData.foreground == false) {
        document.location.href = data.additionalData.path;
      }
      if (data.additionalData.foreground == true ){
        var msg = data.message;
        var alertPopup = $ionicPopup.show({
          title: 'Title',
          template: msg,
          buttons: [
            { text: 'Cancel' },
            {
              text: '<b>Show</b>',
              type: 'button-positive',
              onTap: function(e) {
                return document.location.href = data.additionalData.path;
              }
            }
          ]
        });
      }
    });
  }
});
```
