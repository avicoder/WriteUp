
## 0x00 Background Knowledge

Each Android Application is made up of Activity, Service, Content Provider and Broadcast Receiver, which are the basic components of Android. Among those components, An Activity is the main body of an application and performs most of the display and interaction work. Even to some extent, an “interface” is an Activity.

An activity is a visual user interface on which users can perform some actions. For instance, an Activity can display a menu list for user to choose and some pictures that contain descriptions. A SMS application can include an Activity to display the contacts list as the recipient, an Activity to write messages to the chosen recipient, and an Activity to view recent messages or change settings. Altogether, they compose a cohesive user interface. But each Activity is independent. And each is an implementation of subclass based on Activity parent class.

An application can have one or more Activities, such as the SMS application that we mentioned. Of course, the function and the number of each Activity is naturally determined by the application and its design. Normally, there is always an application marked as the first application that the user would see when starting. If an Activity needs to shift to another, the current Activity will launch the next one.

**Intent Selector**
There is a circumstance that more than one Activity may contain a same action. When such an action is called, a selector will pop up for user to pick.

**Permissions**
android:exported

This property determines whether an Activity component can be launched by external applications. When it's set to true, Activity can be started by an external application. However, when it's set to false, Activity can only be launched by its own app. (same user ID or root can launch)

Exported is set to false by default, if the action property of intent-filter is not configured ( without filter, you have to start the activity by explicit class names, which means the program can only be launched by itself), otherwise, exported is set to true by default.

Exported property is used to limit the Activity not to expose to other apps. Setting the permission statements in configuration files can also limit external apps from starting an Activity.

    android:protectionLevel
    
http://developer.android.com/intl/zh-cn/guide/topics/manifest/permission-element.html

**Normal**: default value. This is a lower risk permission. The system grants this type of permission to a requesting application at install without asking for user's approval.

**Dangerous**: like WRITE_SETTING and SEND_SMS, these permissions are risky, because these permissions can be used to reconfigure the device, or lead charges. This protection Level is used to identify some permissions that the user concerns. Android would display related permissions to the user at install. The specific actions vary from Android versions and devices.

**Signature**: these permissions are only granted to the requesting applications signed with the same certificate as the application that declared the permission

**SignatureOrSystem**: similar to signature class, except for the fact that applications in Android image also require permission. In this case, applications that vendors build in system can still be granted with permission. This protection level is helpful to integrate system compiling process.


## Security recommendations


  - Private Activity used in apps should not configure intent-filter. If intent-filter is configured, exported property needs to be false.
  - Use the default taskAffinity
  - Use the default launchMode
  - Don't set FLAG_ACTIVITY_NEW_TASK when starting an activity
  - Handle the received intent and its information carefully
  - Verify the signature of internal (in-house) app
  - When the Activity returns data , you should pay attention to whether the target Activity would disclose information risks.
  - The target Activity uses explicit start when it's clear.
  - Handle the returned data carefully. The target activity may return data forged by malware.
  - Verify if the target Activity is a malicious app, in case of intent cheat. You can use signature hash to verify.
  - When Providing an Asset Secondhand, the Asset should be Protected with the Same Level of Protection
  - Do not send sensitive information. The intent information of public activity can be read by malware.

## Test Method

**Viewing the activity:**

  - Decompile,and view the activity component in AndroidManifest.xml (pay attention to configured intent-filter and exported is not set to false.
  - Use RE to open the app after install to view the configuration file directly.
  - Use Drozer scan to: run app.activity.info -a packagename
  - Dynamic view: logcat sets the tag of filter to ActivityManager
  
**launching the activity:**
   - adb shell：am start -a action -n package/componet
   - drozer: run app.activity.start --action android.action.intent.VIEW ...
   - Write your own app to call startActiviy () or startActivityForResult ()
   
## Reference :

http://www.jssec.org/dl/android_securecoding_en.pdf
