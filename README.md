<h2><a id="user-content-overview" class="anchor" href="#overview" aria-hidden="true"></a>Overview</h2>

<p>This Package allows you to use the HangIt SDK in Unity 5 mobile applications.</p>

<p>A complete sample class demonstrating the functions of the SDK is available in <code>Assets/Plugins/Hangit/HangitExample.cs</code>.</p>

<p><a id="user-content-include"></a></p>

<h2><a id="user-content-include-the-hangit-library" class="anchor" href="#include-the-hangit-library" aria-hidden="true"></a>Include the HangIt library</h2>

<p><strong><em>Before you begin:</em></strong> <em>This extension requires iOS 8.0 or higher; the plugin will override your target OS if it's set below this. On Android, target API level must be 14+.</em></p>

<p><strong>In Unity:</strong></p>

<ol>
<li>Create a new project and change the platform in Build Settings to iOS or Android.</li>
<li>Select Assets &gt; Import Package &gt; Custom Package.</li>
<li>Click Import on the Window that pops up.</li>
<li>Select the HangitData asset icon under Assets/Resources/Hangit.</li>
<li>Using the Inspector, update the APP ID and Secret Key with the ids from your app created in the Hangit Portal.</li>
<li>[iOS Only] In the Inspector, set your Hangit SDK options for Autostart, Present Notifications and Present Offer View. These are on by default to give the full experience of the Hangit SDK.</li>
<li>We have provided an example under Assets/Plugins/Hangit/HangitExampleScene and HangitExample.cs </li>
</ol>

<p><a id="user-content-integration"></a></p>

<h2><a id="user-content-integration" class="anchor" href="#integration" aria-hidden="true"></a>Integration</h2>

<p>To activate HangIt in your project, you include a few simple function calls:</p>

<h3><a id="user-content-initialize-the-hangit-api" class="anchor" href="#initialize-the-hangit-api" aria-hidden="true"></a>Initialize the HangIt API</h3>

<p>Your will need to create an account and login to <a href="https://portal.hangit.com">https://portal.hangit.com</a> to create an app and generate your App ID Key set in the HangitData asset.</p>

<p>[Android-only] At the very start of your application's execution, initialize the HangIt API by calling <code>Hangit.Init();</code></p>

<pre><code>// start Hangit
void Start()
{
	Hangit.Init(); // will just be ignored on iOS
}
</code></pre>

<h3><a id="user-content-opening-the-wallet" class="anchor" href="#opening-the-wallet" aria-hidden="true"></a>Opening the Wallet</h3>

<p>If your application needs to open the HangIt wallet, call the <code>Hangit.OpenWallet();</code> method at any time after initialization:</p>

<pre><code>// open wallet
Hangit.OpenWallet();
</code></pre>

<h3><a id="user-content-clearing-the-device" class="anchor" href="#clearing-the-device" aria-hidden="true"></a>Clearing the Device</h3>

<p>During testing, you may need to clear the device in order to test a location based notification or offer a second time.  This is done with the <code>Hangit.ClearDevice();</code> function:</p>

<pre><code>// clear device to test location based notifications again
Hangit.ClearDevice();
</code></pre>

<p>If you would like to offer an option for users of your app to pause the Hangit SDK you can call the <code>Hangit.StopLocation();</code> method at any time after initialization (iOS-only):</p>

<pre><code>// stop Hangit
Hangit.StopLocation();
</code></pre>

<p>They can then restart it with <code>Hangit.StartLocation();</code> method at any time after initialization (iOS-only).</p>

<pre><code>// start Hangit again
Hangit.StartLocation();
</code></pre>

<p>Once integration is complete you can go to Build Settings and run and build your Unity app and begin testing our SDK.</p>

NOTE: On Android, several Google Play and Android libraries are added. If you have any duplicates from other plugins, you'll need to delete them. Future versions will use Google's JAR Resolver to manage most of these dependencies and reduce potential conflicts.

