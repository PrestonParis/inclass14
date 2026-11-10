# Firebase Cloud Messaging Lab

CSC 4360 In-Class Activity 14. A Flutter app that receives Firebase Cloud Messaging (FCM) messages and uses their custom data to change the title and image on screen.

## How it works

- `lib/main.dart` initializes Firebase before `runApp()`, registers the top-level background handler, and maps payload data to the UI.
- `lib/fcm_service.dart` owns the messaging listeners, permission request, token handling, and cleanup.
- The screen shows the last handler, the receipt count for this launch, and the last message ID, so each new message is visible even when the content stays the same.

| App state | Event | Handler |
| --- | --- | --- |
| Foreground | Message received | `FirebaseMessaging.onMessage` |
| Background | Notification tapped | `FirebaseMessaging.onMessageOpenedApp` |
| Terminated | Notification tap launches the app | `getInitialMessage()` |
| Background or terminated | Background work, no UI access | `onBackgroundMessage` |

## Payload

```json
{
  "notification": {
    "title": "New promotion",
    "body": "Switch the screen to the promo asset"
  },
  "data": {
    "asset": "promo",
    "action": "update_ui"
  }
}
```

The app accepts `action: update_ui` with `asset: default` or `asset: promo`. Any other value shows a diagnostic and keeps the current title and image.

## Run

You need the Flutter SDK and an Android device or emulator with Google Play services.

```
flutter pub get
flutter run
```

The app logs its registration token to the debug console. Send a test message to that token from the Firebase Console, with `asset` and `action` added as custom data.

`lib/firebase_options.dart` and `android/app/google-services.json` hold client identifiers for the project `mad-ica14-fcm`, not service-account credentials. To use your own project, run `flutterfire configure`.

The app targets Android only and uses `firebase_core` 4.x and `firebase_messaging` 16.x, as the activity requires.
