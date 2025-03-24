---
date: "2024-09-15T00:00:00Z"
title: Reverse Engineering Android Application (EncryptedVault)
tags : ["Andriod Security", "Reverse Engineering",]

---
## Scenario
<p>&nbsp;</p>

You have access to an Android app called EncryptedVault.apk, designed to store confidential information securely using encryption. However, if exploited, multiple flaws in the app's design can grant unauthorized access to sensitive data stored within the app. The app uses a custom encryption algorithm, stores sensitive data in shared preferences, and sends encrypted data to a remote server. Your task is to reverse engineer the app, analyze its encryption scheme, exploit multiple vulnerabilities, and retrieve the hidden flags.

<p>&nbsp;</p>

## Objective

Perform deep reverse engineering to understand the custom encryption algorithm, decrypt the sensitive information stored in the app's local storage, and retrieve flags:

1. **Flag 1**: Extracted from the locally stored encrypted data.
2. **Flag 2**: Hidden inside an obfuscated native library (`libcrypto.so`) and retrieved by analyzing the library.

# Prerequisites

- Download the APK file
- Setup the environment with necessary tools, APKTool, JADX, MOBSF, Frida, Ghidra

# Step 1: Analyze the App’s Behavior

### Initial Review

I started by running the app on an Android emulator to observe its behaviour. This step is crucial to understand how the app works and interacts with native code. From the initial observations, the app seemed to handle encryption and decryption of sensitive data.

To dive deeper, I opened **Logcat** in Android Studio and monitored the app’s logs while using its features. Reviewing the logs helps to pinpoint critical parts of the app's logic, particularly the function calls to native libraries responsible for encryption and decryption.

## Key Insight

The logs provided a valuable starting point for locating the parts of the app that handle sensitive data, which was crucial for later steps when analyzing the encryption scheme and reverse engineering the native libraries.

# Step 2: Reverse engineer the APK

Next, I decompiled the APK using **JADX** to inspect the source code and analyze the app’s functionality. As expected, the app made several calls to native methods, which appeared to handle the encryption tasks.

Upon reviewing the decompiled code, I identified one of the essential methods responsible for encryption. At this stage, I discovered **Flag 1** hidden within the local storage encryption routine.


![image](https://github.com/user-attachments/assets/7ae0e365-1cbf-49e7-8025-5fe5cde6d068)

### Key Insight

Reverse engineering the APK code using JADX exposed the app’s weaknesses, particularly its custom encryption method and the way sensitive data was stored, allowing me to extract the first flag from the locally stored encrypted data.

# Step 2: Investigate Native Code

### Analyzing Native Libraries

To further exploit the vulnerabilities, I needed to investigate the app's native libraries. I used **APKTool** to decompile the APK and obtain the native library files, specifically `libcrypto.so`.

![image](https://github.com/user-attachments/assets/6bc8058b-9946-4d46-b0ee-9e85c8e0847f)

![image](https://github.com/user-attachments/assets/29672837-f010-401d-81f8-f69b7e454627)

I then analyzed `libcrypto.so` using Ghidra, an excellent tool for reverse engineering native code. Ghidra’s decompiler allowed me to explore the library's C++ functions. Following the code flow, I identified the part of the code responsible for handling the encryption.

By tracing the function calls in Ghidra, I could locate the function that generates **Flag 2**. The flag was encoded and stored in a string buffer. After a bit of manual extraction, I successfully retrieved it.

![image](https://github.com/user-attachments/assets/19bab843-8f9b-4c53-9822-9e5fb55516f4)

### Key Insight

Using Ghidra to reverse engineer `libcrypto.so` allowed me to understand the custom encryption process deeper and locate the hidden flag within the obfuscated native library code.

# Conclusion

Through this process, I successfully reverse-engineered the **EncryptedVault.apk** and extracted both flags by:

1. Analyzing the app's behavior and identifying critical points using Logcat.
2. Decompiling the APK with JADX to understand the custom encryption and retrieve **Flag 1**.
3. Analyzing the native library (`libcrypto.so`) with Ghidra to extract **Flag 2**.

This challenge provided valuable insights into reverse engineering Android applications, especially those integrating native code for sensitive operations. It also reinforced the importance of scrutinizing how apps handle encryption, from both Java code and native libraries, to uncover hidden vulnerabilities.



**Tools Used:**
- **Android Studio Emulator** runs the app and observes behaviour.
- **JADX** decomposes the APK and inspects the code.
- **APKTool** to extract the native libraries.
- **Ghidra** is used to analyze native libraries and reverse engineer native code.
- **Frida** for dynamic analysis and function hooking.




