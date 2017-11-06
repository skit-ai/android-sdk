Vernacular.ai Android Sdk
==========================

 [![Download](https://api.bintray.com/packages/axay/maven/chat-sdk/images/download.svg)](https://bintray.com/axay/maven/chat-sdk/_latestVersion)

### Installation

This library is hosted on [bintray](https://bintray.com/axay/maven/chat-sdk/0.3.6) and available on jcenter

    repositories {
        jcenter()
    }


Add this dependency to your build.gradle file :

    compile 'ai.vernacular:chat-sdk:0.4.0'

Or if you are using maven :
```xml
<dependency>
  <groupId>ai.vernacular</groupId>
  <artifactId>chat-sdk</artifactId>
  <version>0.4.0</version>
  <type>pom</type>
</dependency>
```
### Usage instructions

#### Initialize the SDK

In the application class :

```java
package package.name;

import android.app.Application;
import ai.vernacular.core.VernacularSdk;


public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();

        VernacularSdk.init(this, "YOUR_ACCESS_TOKEN");
    }
}
```

Make sure to add this class to `AndroidManifest.xml`

```xml
<application
    android:name=".App">
    ...

    <activity
            android:name="ai.vernacular.ui.VernacularChatActivity" />
</application>
```
#### Displaying the chat user interface

```java
VernacularChatActivity.start(this, "chat_activity_title");
```
#### [Optional] Add user profile

If you have the basic attributes of the user, you can customize how they are visible in your dashboard
```java
User user = User.init(this)
             .setUserId("unique_user_id")
             .setFirstName("Jane")
             .setLastName("Doe");
```
You can also set custom properties for your user

```java
user.setCustomProperty("custom_field_key", "custom_field_value");
```

And finally call login before starting the activity,

```java
VernacularSdk.login(user);
```

#### [Optional] Add FCM Notifications

**Replace** the dependency in build.gradle with fcm one :

    compile 'ai.vernacular:chat-sdk-fcm:0.4.0'

Or if you are using maven :
```xml
<dependency>
  <groupId>ai.vernacular</groupId>
  <artifactId>chat-sdk-fcm</artifactId>
  <version>0.4.0</version>
  <type>pom</type>
</dependency>
```

And in `AndroidManifest.xml` :

```xml
<activity
            android:name="ai.vernacular.ui.VernacularChatActivity">
            <intent-filter>
                <action android:name="ai.vernacular.chat.open"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
```
#### [Optional] Using Vernacular FCM Sdk with other FCM setups
This only applies to applications that also use FCM for their own content, or use a third party service for FCM. You’ll need to update your **FirebaseInstanceIdService** and **FirebaseMessagingService**.

You should have a class that extends FirebaseInstanceIdService. That service is where you get the device token to send to your backend to register for push. To register for Vernacular Sdk notifications set up your FirebaseInstanceIdService like this:

```java
private VernacularPushClient vernacularPushClient = new VernacularPushClient();

@Override public void onTokenRefresh() {
    String refreshedToken = FirebaseInstanceId.getInstance().getToken();
    
    // SEND TOKEN TO OUR CLIENT
    vernacularPushClient.onTokenRefresh(refreshedToken);

    //DO YOUR LOGIC HERE
}
```

You should have a class that extends FirebaseMessagingService. To allow us to listen for our messages set up your FirebaseMessagingService like this:

```java
private VernacularPushClient vernacularPushClient = new VernacularPushClient();

public void onMessageReceived(RemoteMessage remoteMessage) {
    Map message = remoteMessage.getData();
    if (vernacularPushClient.isVernacularMessage(message)) {
        vernacularPushClient.onMessageReceived(getApplication(), message);
    } else {
        //DO YOUR LOGIC HERE
    }
}
```

To get firebase console server key follow the instructions on [this](https://github.com/Vernacular-ai/android-sdk/wiki/FCM-Notifications) page.

#### [Optional] Theme VernacularChatActivity

```xml
<style name="VernacularChatTheme" parent="Theme.AppCompat">
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
    <item name="android:textSize">14sp</item>
    <item name="android:fontFamily">sans-serif</item>
    <item name="windowActionBar">true</item>
    <item name="windowNoTitle">false</item>
</style>
 ```

Add this style to VernacularChatActivity in AndroidManifest.xml

```xml
<activity
    android:name="ai.vernacular.ui.VernacularChatActivity"
    android:theme="@style/VernacularChatTheme" />
```

### License

    Copyright 2017 Cyllid Technologies Pvt. Ltd.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
    
