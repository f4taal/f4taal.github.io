---
date: "2024-02-10T00:00:00Z"
title: Hooking Android Broadcast Receivers with Frida
tags : ["Andriod Security", "Debugging", "Frida"]

---
## Introduction
<p>&nbsp;</p>

In this blog, we will explore Android Broadcast Receiver, and discover the nuances of intercepting broadcast receivers using Frida. we'll delve into practical insights with minimal prerequisites.

<p>&nbsp;</p>

# Prerequisites

- InsecureBankv2 APK installed in our emulator
- Properly configured Frida setup.

# Andriod Broadcast Receivers

### What is Broadcast Receivers?

Android Broadcast Receivers are components that simply respond to broadcast messages events or intents from another application or from the system itself. Hooking broadcast receivers will involve intercepting function calls, events, or messages passed between software components of Android. This article will guide you through the process of hooking Android broadcast receivers using Frida and identifying the vulnerabilities that malicious actors may exploit. We'll also discuss the potential security implications of this practice.

> Ensure that your device is properly configured for debugging, and the USB Debugging option is enabled for a smooth connection, identify and start the target application (we are using the
[InsecureBankv2](https://github.com/dineshshetty/Android-InsecureBankv2/blob/master/InsecureBankv2.apk) application) for the demo.

## Scripting: Hooking Broadcast Receivers with Frida

### List Available Broadcast Receivers

First, we need to check which receivers are available in our target. To achieve this we can either create a script to extract the information or use `adb` to get similar information, we can use adb instead by running, 

```bash
adb shell dumpsys package com.android.insecurebankv2 --include-all 
```

![image](/img/BR-Frida1/frida3-3.png)

Given that ``MyBroadCastReceiver`` processes actions with the name ``theBroadcast`` and is exported without protection by permission, any app can create an Intent triggering this receiver. 

## Exploiting Vulnerabilities

### Analyzing Source Code

View the source code using tools like Bytecode Viewer to understand the receiver's capabilities, as illustrated in the accompanying image.

![image](/img/BR-Frida1/frida3-4.png)

After observing, the source code reveals that the receiver retrieves two parameters, `phone number` and `newpass` from the incoming Intent. Subsequently, the code accesses data stored in Shared Preferences, conducts cryptographic operations and concludes by invoking the SmsManager.sendTextMessage() method.

### Exploitation Script 

Create a Frida script to trigger Broadcast Receivers and potentially exploit vulnerabilities.

*Script Content*

```bash
Java.perform(function () {
    try {
        var Intent = Java.use('android.content.Intent');
        var Application = Java.use('android.app.Application');
        var MyBroadCastReceiver = Java.use('com.android.insecurebankv2.MyBroadCastReceiver');
        var SmsManager = Java.use('android.telephony.SmsManager');
        var intent = Intent.$new();
        intent.setAction('theBroadcast');
        intent.putExtra('message', 'Hello from Frida!');
        var currentApplication = Java.use('android.app.ActivityThread').currentApplication();
        var context = currentApplication.getApplicationContext();
        var smsManager = SmsManager.getDefault();
        var phoneNumber = '12345';
        var message = 'This is a test message from Frida.';
        smsManager.sendTextMessage(phoneNumber, null, message, null, null);
        console.log('SMS sent successfully');
        context.sendBroadcast(intent);
        console.log('Broadcast triggered successfully');
    } catch (error) {
        console.error('Error in Frida script:', error);
    }
    
});

```

Save the script and name it list.js, now run;

```bash
frida -U -l list.js -f com.android.insecurebankv2
```

![image](/img/BR-Frida1/frida3-5.png)

![image](/img/BR-Frida1/frida3-6.png)

*SMS sent successfully*

![image](/img/BR-Frida1/frida3-7.png)

After running our script, we noticed on our console that the broadcast was triggered successfully. and the popup on our application. 

**What does our script do?**

- **Import Classes** The script imports several classes from the Android framework that it will use later. These include Intent, Application, `MyBroadCastReceiver`, and `SmsManager`.
- **Create an Intent** An Intent object is created with the action `theBroadcast` and an extra message `Hello from Frida!`. Intents are used in Android to request an action from another app component.
- **Get Application Context** The script retrieves the current application context using the `ActivityThread.currentApplication() method.` The application context is needed to interact with the application's environment, such as sending broadcasts or accessing resources.
- **Send an SMS** The script uses the `SmsManager` class to send an SMS message. It specifies a phone number `(12345)` and a message `(This is a test message from Frida.)`. The sendTextMessage method is called with null values for the sent and delivery pending intents, meaning no callbacks will be made regarding the status of the SMS.
- **Log Successful SMS** After sending the SMS, the script logs a message to the console indicating that the `SMS was sent successfully`.
- **Trigger a Broadcast** The script then triggered a broadcast using the previously created Intent. This broadcast is sent to all components that have registered to receive it with the action `theBroadcast`.
- **Log Successful Broadcast** Finally, the script logs a message to the console indicating that the broadcast was triggered successfully.
- **Error Handling** If any errors occur during the execution of the script, they are caught and logged to the console.

This script demonstrates how Frida can be used to interact with an Android application at runtime. It shows how to create and send intents, trigger broadcasts, and send SMS messages. 

We have used Frida to hook into our target Android application `com.android.insecurebankv2`. The script attempts to intercept and manipulate the behavior of a `BroadcastReceiver` within the application.

## Security Implications

From the results we got by attaching our script to the target in our demo, we can infer that the following types of vulnerabilities were potentially targeted;

**Improper Access Control** If the `BroadcastReceiver` is accessible without proper authentication or authorization checks, an attacker could intercept and manipulate broadcasts intended for secure communication.

**Unvalidated Input** If the `BroadcastReceiver` processes input data without proper validation, an attacker could craft malicious intents with harmful payloads.

**Insecure Direct Object References (IDOR)** If the `BroadcastReceiver` uses references to internal objects without proper access controls, an attacker could potentially access sensitive data or perform unauthorized actions.

**Insecure Inter-Process Communication (IPC)** If the application allows unrestricted inter-process communication, an attacker could send specially crafted intents to affect the behavior of the application.

*The exploitation process in this case involves the following steps*

**Hooking** Using Frida, the script hooks into the application's process to intercept calls to the `BroadcastReceiver`'s `onReceive` method.

**Overriding Behavior** The script replaced the `onReceive` method with a custom implementation that logged the received intent and optionally triggered additional actions.

**Triggering the Broadcast** The script creates a new intent with a specified action and extras and then sends this broadcast using the application's context.

**Intercepting the Response** By controlling the `onReceive` method, the script observed and potentially altered the response to the broadcast.

## Conclusion

In conclusion, this blog post explored the process of hooking Android Broadcast Receivers using Frida, identified and exploited the vulnerabilities, and discussed security implications. Stay tuned for more insights in my upcoming posts.

## Tools Used

- Frida
- Android Studio with Andriod Emulator running
- Basic understanding of Android development and Java programming language

## References

* [Broadcat Receiver in Andriod with example](https://www.geeksforgeeks.org/broadcast-receiver-in-android-with-example/)

* [Broadcast Overview | Background work](https://developer.android.com/develop/background-work/background-tasks/broadcasts#java/)


