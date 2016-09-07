# Hangit Native Package for Unity Guide #

<!--- This guide has the following sections:

* [Overview](#overview)
* [Include the HangIt Library](#include)
* [Update Your Application Descriptor](#descriptor)
* [ActionScript Integration](#integration)

-->

<a id="overview"></a>
## Overview ##

This Package allows you to use the HangIt SDK in Unity mobile applications.

A complete sample class demonstrating the functions of the API is available in the `/library` folder.

<a id="include"></a>
## Include the HangIt library ##

***Before you begin:*** *This extension requires iOS 8.0 or higher.*

**In Unity:**

1. Create a new project of the type for iOS Or Android.
2. Select Assets > Import Package > Custom Package.
3. Select the HangitData asset icon under Resources/Hangit.
4. Using the Inspector update the APP ID with the ID from your app created in the Hangit Portal.
5. Using the Inspector set your Hangit SDK options for Autostart, Present Notifications and Present Offer View. These are on by default to give the full experience of the Hangit SDK.
6. We have provided an example under /iOS/Hangit/HangitExample.cs 

### For Projects Targeting iOS ###

To use the Package for iOS, you'll need to include the *NSAppTransportSecurity* data show below into your Application, set the `MinimumOSVersion` to 8.0, and include the required `UIBackgroundModes` and `NSLocationWhenInUseUsageDescription` and `NSLocationAlwaysUsageDescription` values:

	<InfoAdditions>
		<![CDATA[
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key><true/>
	</dict>

    <key>MinimumOSVersion</key>
    <string>8.0</string>

    <key>UIBackgroundModes</key>
    <array>
         <string>location</string>
       	<string>fetch</string>
    </array>

	<key>NSLocationWhenInUseUsageDescription</key>
	<string>We offer great deals at places near you, so let us know where you are. We’ll never spam you.</string>
	<key>NSLocationAlwaysUsageDescription</key>
	<string>We offer great deals at places near you, so let us know where you are. We’ll never spam you.</string>
	]]>
	</InfoAdditions>

### For Projects Targeting Android ###

In order to use the Package on Android, you'll need to update the `manifestAdditions` block in your your `application.xml` file (for convenience, it may easier to copy and paste this section from the `example/app.xml` file included with the Package):

 	<android>
        <manifestAdditions><![CDATA[
			<manifest android:installLocation="auto">

				<!-- These permissions are required by HangIt -->
				<uses-permission android:name="android.permission.INTERNET" />
				<uses-permission android:name="android.permission.READ_PHONE_STATE" />
				<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
				<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
				<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
				<uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />
				<uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION" />
				<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

				<application>

				<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />


       <service android:name="com.hangit.android.hangit_sdk.ServiceHangITLocation">
            <meta-data android:name="com.hangit.android.hangit_sdk.notification_icon" android:value="ic_launcher"/>
            <meta-data android:name="com.hangit.android.hangit_sdk.notification_activity" android:value="com.hangit.android.hangit_sdk.UIWebViewActivity"/>
            <meta-data android:name="com.hangit.android.hangit_sdk.appnext_notification_activity" android:value="com.hangit.android.hangit_sdk.UIAppNextActivity"/>
            <meta-data android:name="com.hangit.android.hangit_sdk.notification_back_activity" android:value="com.hangit.android.hangit_sdk.NotificationBackActivity"/>
        </service>
        <service android:name="com.hangit.android.hangit_sdk.ServiceActivityRecognition"/>
        <service android:name="com.hangit.android.hangit_sdk.ServiceBeacon"/>
        <receiver android:name="com.hangit.android.hangit_sdk.ReceiverBootup">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="android.intent.action.QUICKBOOT_POWERON"/>
            </intent-filter>
        </receiver>

        <activity android:name="com.hangit.android.hangit_sdk.UIWebViewActivity"/>
        <activity android:name="com.hangit.android.hangit_sdk.UIMapActivity" android:screenOrientation="portrait"/>
        <activity android:name="com.hangit.android.hangit_sdk.UIBackgroundActivity" android:screenOrientation="portrait"/>
        <activity android:name="com.hangit.android.hangit_sdk.UINotificationBackActivity"/>
        <activity android:name="com.hangit.android.hangit_sdk.UIAppNextActivity" android:screenOrientation="portrait"/>
        <activity android:name="com.hangit.android.hangit_sdk.UIWalletActivity" android:screenOrientation="portrait"/>

        <!--meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="AIzaSyA6anZrDFdlkwkWN41s73rYnzB-INSIgD0"/-->

        <activity android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|screenSize|smallestScreenSize|uiMode" android:name="com.google.android.gms.ads.AdActivity" android:theme="@android:style/Theme.Translucent"/>
        <activity android:name="com.google.android.gms.ads.purchase.InAppPurchaseActivity" android:theme="@style/Theme.IAPTheme"/>

        <meta-data android:name="com.google.android.gms.wallet.api.enabled" android:value="true"/>
        <receiver android:exported="false" android:name="com.google.android.gms.wallet.EnableWalletOptimizationReceiver">
            <intent-filter>
                <action android:name="com.google.android.gms.wallet.ENABLE_WALLET_OPTIMIZATION"/>
            </intent-filter>
        </receiver>
        <service android:exported="false" android:name="com.estimote.sdk.service.BeaconService"/>


				</application>
			</manifest>			
		]]></manifestAdditions>
    </android>

<a id="integration"></a>
## Integration ##

To activate HangIt in your project, you include a few simple function calls:

### Initialize the HangIt API ###

Your will need to create an account and login to https://portal.hangit.com to create an app and generate your App ID Key set in the HangitData asset.

 At the very start of your application's execution, initialize the HangIt API by calling `Hangit.StartLocation();`.

  // start Hangit
  Hangit.StartLocation();
  
### Opening the Wallet ###

If your application needs to open the HangIt wallet, call the `Hangit.OpenWallet();` method at any time after initialization:

	// open wallet
	Hangit.OpenWallet();

### Clearing the Device ###

During testing, you may need to clear the device in order to test a location based notification or offer a second time.  This is done with the `Hangit.ClearDevice();` function:

		// clear device to test location based notifications again
		Hangit.ClearDevice();

If you would like to offer an option for users of your app to pause the Hangit SDK you can call the `Hangit.StopLocation();` method at any time after initialization:

  // stop Hangit
  Hangit.StopLocation();
  
Once integration is complete you can go to Build Settings and run and build your Unity app and begin testing our SDK

