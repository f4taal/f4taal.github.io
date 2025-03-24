---
date: "2024-09-15T00:00:00Z"
title: Reverse Engineering Android Application
tags : ["Andriod Security", "Reverse Engineering",]

---
## Scenario
<p>&nbsp;</p>

You’ve intercepted a suspicious Android app, that contains secret data. Your task is to reverse-engineer the app, uncover hidden data, and retrieve the flag. To solve this challenge, you’ll need to demonstrate your reverse engineering and basic Android penetration testing skills.

<p>&nbsp;</p>

## Objective
Find the flag hidden within the APK by reverse-engineering it

# Prerequisites

- Download the APK file
- Install APKTool for decompiling the APK
- Install JADX For reverse engineering and inspecting the Java code.

# Decompile the APK using APKTool

Decompile the APK file and explore.

```bash
apktool d app-debug.apk
```
![image](https://github.com/user-attachments/assets/cdc4fac3-2db5-4de7-a525-02d252c86215)


#  Reverse Engineering the APK file

Use JADX to open the APK file

```bash
jadx-gui app-debug.apk
```
Navigate through the app’s MainActivity and resources. 
Have checked the strings.xml, to get the flag 

![image](https://github.com/user-attachments/assets/901a80ff-1aa5-4f4a-b748-9da51b6c59ab)


## Conclusion

In conclusion, this blog post has explored how to use the specified tools decompile, analyze, and reverse engineer Android application to test the basic Android penetration testing skills.



