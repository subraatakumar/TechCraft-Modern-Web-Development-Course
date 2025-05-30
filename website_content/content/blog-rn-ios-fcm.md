# 🚀 **iOS Firebase Push Notifications — Step-by-Step**

---

## 1. 🔧 **Firebase Console Setup**

### ✅ Create iOS App in Firebase

* Go to **Firebase Console** → **Your Project** → **Project Settings** → **General** tab.
* Under **Your apps**, click on **Add app** → **iOS** (`</>`).
* Enter:

  * **iOS bundle ID** (exactly same as Xcode bundle ID).
  * (Nickname is optional).
  * **App Store ID** is optional (for now).
* **Download `GoogleService-Info.plist`** — drag it into your Xcode project (`Runner`/`YOUR_APP` root).

---

### ✅ Enable Cloud Messaging

* **Firebase Console** → **Project Settings** → **Cloud Messaging**.
* Scroll down to **Apple app configuration**.
* Under **APNs Authentication Key**:

  * Upload your `.p8` key.
  * Enter your **Key ID** and **Team ID** from the Apple Developer account.

> **If you don't have a .p8 key yet:**
>
> * Go to [Apple Developer → Certificates, IDs & Profiles → Keys](https://developer.apple.com/account/resources/authkeys/list).
> * Create a new Key → Enable `Apple Push Notifications Service (APNs)`.
> * Download the `.p8` file.
> * Copy the **Key ID** and **Team ID**.

---

## 2. 🍎 **Apple Developer Portal Setup**

### ✅ App ID Configuration

* Go to [Apple Developer → Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/identifiers/list).
* **Identifiers** → Select your App → Enable **Push Notifications**.

---

### ✅ Create Provisioning Profile

* Go to **Profiles** → Create or Edit your **Provisioning Profile**.
* Ensure the App ID associated has **Push Notifications** enabled.
* Download the updated profile and use it in Xcode if needed.
* OR turn **Automatic signing ON** in Xcode to auto-manage profiles.

---

## 3. ⚙️ **Xcode Project Setup**

### ✅ Enable Capabilities

In Xcode:

* Open your project.
* Click your app target → **Signing & Capabilities** tab.
* Click `+ Capability` → Add:

  * ✅ **Push Notifications**
  * ✅ **Background Modes**

    * Enable `Remote notifications`.

✅ **Ensure:**

* **Automatic Signing** is enabled (or use correct profile manually).
* **GoogleService-Info.plist** is included in your app target.

---

### ✅ Update AppDelegate.m

If you have disabled Firebase automatic handling:

In `AppDelegate.m`:

```objc
#import <Firebase.h> // Add at top

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  [FIRApp configure];
  // your existing code
  return YES;
}
```

If you haven't disabled `FirebaseAppDelegateProxyEnabled`, you don't need extra delegate methods.

---

### ✅ Info.plist

Nothing special is needed unless:

* You want to handle notifications yourself — then set:

```xml
<key>FirebaseAppDelegateProxyEnabled</key>
<false/>
```

Otherwise, **no change** is needed.

---

## 4. ⚛️ **React Native (JavaScript) Setup**

### ✅ Install Dependencies

```bash
npm install --save @react-native-firebase/app @react-native-firebase/messaging
cd ios && pod install && cd ..
```

---

### ✅ Request Permissions

In your JS code (`App.tsx` or a notification handler file):

```tsx
import messaging from '@react-native-firebase/messaging';
import { Alert, Platform } from 'react-native';

const requestPermission = async () => {
  const authStatus = await messaging().requestPermission();
  const enabled =
    authStatus === messaging.AuthorizationStatus.AUTHORIZED ||
    authStatus === messaging.AuthorizationStatus.PROVISIONAL;

  if (enabled) {
    console.log('Notification permission enabled');
  } else {
    console.log('Notification permission denied');
  }
};
```

Call `requestPermission()` during app startup (`useEffect`).

---

### ✅ Get the FCM Token

```tsx
const getFcmToken = async () => {
  try {
    if (Platform.OS === 'ios') {
      await messaging().registerDeviceForRemoteMessages(); // Not always necessary if auto-register is enabled.
    }
    const token = await messaging().getToken();
    console.log('FCM Token:', token);
  } catch (error) {
    console.error('Error getting FCM token:', error);
  }
};
```

---

## 5. 📲 **Handling Notifications**

### ✅ Foreground Notifications — Manual Handling

Add this listener:

```tsx
useEffect(() => {
  const unsubscribe = messaging().onMessage(async remoteMessage => {
    console.log('Foreground notification:', remoteMessage);
    Alert.alert(
      remoteMessage.notification?.title ?? 'Notification',
      remoteMessage.notification?.body ?? 'You have a new message.'
    );
  });

  return unsubscribe;
}, []);
```

* **Background & Killed**: iOS will automatically show notification banner — **only if `notification` block** is present in payload.
* **Foreground**: iOS needs you to show an Alert, or a custom notification UI.

---

## 6. 🔥 **Sending Notifications**

✅ **Your payload MUST have**:

```json
{
  "to": "DEVICE_FCM_TOKEN",
  "notification": {
    "title": "Hello",
    "body": "This is a notification."
  },
  "data": {
    "key1": "value1"
  }
}
```

* `notification` block is required for iOS to show push notifications.
* `data` block is optional but useful if you want additional information.

---

## 7. 🛠️ **Common Issues Checklist**

| Check                                  | Description                       |
| -------------------------------------- | --------------------------------- |
| Firebase APNs Key Uploaded             | In Firebase Console               |
| Push Notification Capability           | Added in Xcode                    |
| Background Mode (Remote Notifications) | Enabled in Xcode                  |
| Correct Provisioning Profile           | Push Notification enabled profile |
| Request Permissions                    | In JS Code                        |
| Foreground Notification Handling       | `messaging().onMessage()`         |
| Payload                                | Contains `notification` block     |

---

# ✅ **Once All Set**

* Build the app **on real device** (NOT simulator — push notifications don’t work on simulator).
* Check FCM Token in console.
* Send a notification via Firebase Console or server with valid payload.

---

# 🚀 **You should now:**

* Get a notification in background / killed state automatically.
* Get notification manually shown when app is foreground.

