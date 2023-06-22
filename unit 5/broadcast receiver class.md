In Android Development, the BroadcastReceiver class is a fundamental component that enables communication between different parts of an application or between different applications on an Android device. It allows the application to listen for and respond to broadcast messages sent by the system or other applications. This class is primarily used to handle events or notifications in an asynchronous manner.

## Use Case:

The BroadcastReceiver class is commonly used in scenarios where an application needs to respond to system events, such as when the device boots up, the network connectivity changes, the battery level changes, an SMS message is received, or a notification is posted. By registering a BroadcastReceiver, the application can be notified of these events and perform specific actions in response.

There are mainly two types of Broadcast Receivers:

1. _Static Broadcast Receivers:_ These types of Receivers are declared in the manifest file and works even if the app is closed but due to the security reasons this is stoped from API Level 26.
2. _Dynamic Broadcast Receivers:_ These types of receivers work only if the app is active or minimized.

Following are some of the important system-wide generated intents:-

| Intent                                          | Description                                                     |
| ----------------------------------------------- | --------------------------------------------------------------- |
| android.intent.action.BATTERY_LOW               | Indicates low battery condition on the device.                  |
| android.intent.action.BATTERY_OKAY              | Indicates the battery level has returned to normal.             |
| android.intent.action.SCREEN_ON                 | Indicates the device's screen has turned on.                    |
| android.intent.action.SCREEN_OFF                | Indicates the device's screen has turned off.                   |
| android.intent.action.ACTION_POWER_CONNECTED    | Indicates the device is connected to a power source.            |
| android.intent.action.ACTION_POWER_DISCONNECTED | Indicates the device is disconnected from a power source.       |
| android.intent.action.ACTION_BOOT_COMPLETED     | Indicates the device has completed the boot process.            |
| android.intent.action.TIME_TICK                 | Indicates a minute has passed and the system clock has changed. |

## How to create a BroadcastReceiver class?

To create a BroadcastReceiver class, you need to create a class that extends the BroadcastReceiver class and override its `onReceive()` method. This method will be called whenever a broadcast message is received by the device. You can then perform your desired actions in this method.

```java
public class MyReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        // Perform your desired actions here
    }
}
```

## How to register a BroadcastReceiver?

To use the BroadcastReceiver class, you need to register it either dynamically or statically in your AndroidManifest.xml file. Here's an example of registering the receiver dynamically:

```java
// Inside your activity or service
MyReceiver myReceiver = new MyReceiver();
IntentFilter intentFilter = new IntentFilter();
intentFilter.addAction(Intent.ACTION_POWER_CONNECTED);
intentFilter.addAction(Intent.ACTION_POWER_DISCONNECTED);
registerReceiver(myReceiver, intentFilter);
```

Once the receiver is registered, whenever a broadcast message is received by the device, the `onReceive()` method of `MyReceiver` will be invoked. You can then perform your desired actions, such as displaying a notification, processing the message, or updating the UI.

## How to unregister a BroadcastReceiver?

To unregister a BroadcastReceiver, you need to call the `unregisterReceiver()` method and pass the BroadcastReceiver object as an argument. For example:

```java
// Inside your activity or service
unregisterReceiver(myReceiver);
```

## How to send a broadcast message?

To send a broadcast message, you need to create an Intent object and pass the action name as an argument. For example:

```java
Intent intent = new Intent(Intent.ACTION_POWER_CONNECTED);
sendBroadcast(intent);
```

You can also pass additional data with the Intent object. For example:

```java
Intent intent = new Intent(Intent.ACTION_POWER_CONNECTED);
intent.putExtra("message", "Device is connected to a power source");
sendBroadcast(intent);
```

## Example:

Let's consider an example where you want to build an application that responds to incoming SMS messages.
**_step1:_** create a class that extends the BroadcastReceiver class and override its `onReceive()` method. This method will be called whenever an SMS message is received by the device.
**SMSReceiver.java**

```java
public class SMSReceiver extends BroadcastReceiver {
    private static final String TAG = "SMSReceiver";
    @Override
    public void onReceive(Context context, Intent intent) {
        if (intent.getAction().equals(Telephony.Sms.Intents.SMS_RECEIVED_ACTION)) {
            // Retrieve the SMS message from the intent
            Bundle bundle = intent.getExtras();
            if (bundle != null) {
                //"pdus" stands for "Protocol Data Units." It is the industry format for an SMS message
                Object[] pdus = (Object[]) bundle.get("pdus");
                if (pdus != null) {
                    // Process each SMS message
                    for (Object pdu : pdus) {
                        SmsMessage sms = SmsMessage.createFromPdu((byte[]) pdu);
                        String message = sms.getMessageBody();
                        String sender = sms.getOriginatingAddress();

                        // Perform your desired actions with the received SMS
                        // For example, you can display a notification or save the message to a database
                        displayNotification(context, sender, message);
                    }
                }
            }
        }
    }

    private void displayNotification(Context context, String sender, String message) {
        // Code to display a notification to the user
        // You can use the NotificationCompat.Builder class to build and show a notification
        Log.d(TAG, "displayNotification: "+sender+" "+message);
        Toast.makeText(context, sender+" sends: "+message, Toast.LENGTH_SHORT).show();
    }
}
```

**step2:**
To use the `SMSReceiver` class, you need to register it either dynamically or statically in your AndroidManifest.xml file. Here's an example of registering the receiver dynamically:

```java
// Inside your activity or service
SMSReceiver smsReceiver = new SMSReceiver();
IntentFilter intentFilter = new IntentFilter(Telephony.Sms.Intents.SMS_RECEIVED_ACTION);
registerReceiver(smsReceiver, intentFilter);
```

Once the receiver is registered, whenever an SMS message is received by the device, the `onReceive()` method of `SMSReceiver` will be invoked. You can then perform your desired actions, such as displaying a notification, processing the message, or updating the UI.

**step3:**
You also need the permission to read user incomming sms messages. To request permissions at runtime in an Android application, you need to follow these steps:

A. Declare the permissions in the AndroidManifest.xml file, as mentioned in the previous response.

```xml
<uses-permission android:name="android.permission.RECEIVE_SMS" />
```

B. Check if the required permissions are granted or not before performing any actions that require those permissions. You can use the `checkSelfPermission()` method to check the status of a permission. For example:

```java
if (ContextCompat.checkSelfPermission(this, Manifest.permission.RECEIVE_SMS)
        != PackageManager.PERMISSION_GRANTED) {
    // Permission is not granted, request it
    ActivityCompat.requestPermissions(this,
            new String[]{Manifest.permission.RECEIVE_SMS},
            MY_PERMISSIONS_REQUEST_RECEIVE_SMS);
} else {
    // Permission is already granted, proceed with your code
    // ...
}
```

C. Implement the `onRequestPermissionsResult()` method in your activity to handle the permission request result:

```java
   @Override
   public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
       if (requestCode == MY_PERMISSIONS_REQUEST_RECEIVE_SMS) {
           if (grantResults.length > 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
               // Permission granted, proceed with your code
               // ...
           } else {
               // Permission denied, handle accordingly (e.g., show an explanation or disable functionality)
               // ...
           }
       }
   }
```

When you call `requestPermissions()`, the system will display a dialog to the user asking for permission. The user can choose to grant or deny the permission. Once the user responds to the permission request, the `onRequestPermissionsResult()` method will be called with the result. You can handle the result based on whether the permission was granted or denied.

Remember to replace `MY_PERMISSIONS_REQUEST_RECEIVE_SMS` with your desired request code. This code is used to identify the specific permission request in the `onRequestPermissionsResult()` method.

**step4:**
Remember to unregister the receiver when you no longer need to listen for SMS messages:

```java
// Inside your activity or service
unregisterReceiver(smsReceiver);
```

This is just one example of how the BroadcastReceiver class can be used in Android Java. It allows you to listen for a wide range of system or application events and respond to them accordingly.

## localBroadcastManager

The `LocalBroadcastManager` class is used to send and receive broadcasts within your application. It is similar to the `BroadcastReceiver` class, but it is more efficient and secure. It is recommended to use `LocalBroadcastManager` instead of `BroadcastReceiver` whenever possible.

To use the `LocalBroadcastManager` class, you need to follow these steps:

1. Define a `BroadcastReceiver` class to handle the broadcast:

```java
public class MyReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        // Handle the broadcast
        String message = intent.getStringExtra("message");
        Log.d("MyReceiver", "Received broadcast: " + message);
    }
}
```

2. Register the `BroadcastReceiver` in your activity or fragment:

```java
public class MyActivity extends AppCompatActivity {
    private MyReceiver myReceiver;
    private LocalBroadcastManager localBroadcastManager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // Initialize LocalBroadcastManager
        localBroadcastManager = LocalBroadcastManager.getInstance(this);

        // Create an instance of MyReceiver
        myReceiver = new MyReceiver();

        // Register the receiver with intent filter
        IntentFilter intentFilter = new IntentFilter("my_custom_action");
        localBroadcastManager.registerReceiver(myReceiver, intentFilter);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();

        // Unregister the receiver
        localBroadcastManager.unregisterReceiver(myReceiver);
    }

    // Method to send a local broadcast
    private void sendLocalBroadcast() {
        Intent intent = new Intent("my_custom_action");
        intent.putExtra("message", "Hello, local broadcast!");
        localBroadcastManager.sendBroadcast(intent);
    }
}
```

3. Send a local broadcast from any part of your code:

```java
// Call this method to send a local broadcast
sendLocalBroadcast();
```

> note: Remember to add the necessary permissions and register the activity in the manifest file.

## LocalBroadcastManager and BroadcastReceiver

1. `BroadcastReceiver`:

   - A class that allows your application to receive and handle broadcast intents.
   - It can be registered either dynamically in code or statically in the manifest file.
   - It receives broadcasts based on the intent filters specified during registration.
   - It overrides the `onReceive()` method to define the actions to be taken when a broadcast is received.

2. `LocalBroadcastManager`:
   - A helper class that allows you to send and receive broadcasts within your application.
   - It operates within the application's process and is not visible to other applications.
   - It provides a mechanism for efficient communication between components within the same application.
   - It simplifies the process of sending and receiving broadcasts by limiting the scope to the local application.

A comparison between `LocalBroadcastManager` and `BroadcastReceiver`:

| Aspect        | LocalBroadcastManager                               | BroadcastReceiver                                    |
| ------------- | --------------------------------------------------- | ---------------------------------------------------- |
| Scope         | Restricted to the local application                 | Can be used across applications                      |
| Broadcasting  | Can send broadcasts within the application          | Does not send broadcasts itself                      |
| Receiving     | Can receive broadcasts within the application       | Receives broadcasts from any sender                  |
| Communication | Efficient communication within the same application | Enables communication between different applications |
| Registration  | No need to register in the manifest file            | Can be registered in the manifest file               |
| Dependency    | Requires the LocalBroadcastManager class            | Does not depend on the LocalBroadcastManager class   |
| Customization | Provides limited customization options              | Offers more flexibility in customization             |
