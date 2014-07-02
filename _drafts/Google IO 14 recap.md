Google I/O 14 recap

tl;dr: New Design, New Android version in preview, Android everywhere, Wear smartwatches available

Material Design
http://googledevelopers.blogspot.com/2014/06/this-is-material-design.html
This is the new design Google will push throughout its applications. From smartwatches to TV, from the web to mobile apps.
http://youtu.be/Q8TXgCzxEnw
It reacts to user input, it has big imagery and typography, bold colors. It has some similarities with Windows 8 "Metro" UI, but it really feels more natural.
If you want to use it now on web projects, you can use Polymer, the framework made by Google using Web Components. There is a nice example http://www.polymer-project.org/apps/topeka/ of a web app using Material Design. And there is even a game Google copied on us: http://smartypins.withgoogle.com/


Android
Sundar Pichai, the boss of Chrome and Android, said that there are now 1 billion active users each month.
http://www.businessinsider.com/google-we-have-1-billion-monthly-active-android-users-2014-6
And they are aiming for the next billion in emerging countries, with a project called Android One http://www.theverge.com/2014/6/26/5845562/android-one-google-the-next-billion. Its purpose is to give guidelines to manufacturers to build low-cost Android phones.

Android L
	First, Android L is getting a facelift with a Material Design theme.
	It brings notifications to the lockscreen, letting you decide if you want to show the whole notification text, or just the application name. There is also a new notification type: "heads-up" notifications show in pop-up window above your current application. Thankfully, it seems to be handled better than it is in iOS.
	Better battery life is promised, with the launch of a new "Projet Volta". Its aim is to give you more info on what uses your battery, and enable developpers to batch background tasks under certain conditions.
	Bye bye Dalvik, ART is now the default runtime. Introduced in beta in KitKat, it promises more speed for your apps with ahead-of-time compilation and a more efficient garbage collection. Couple that with 64 bits support and we should have a very speedy Android L.
	There are more new stuff in Android L, including Bluetooth 4.1 support, Android Extension Pack for high performance graphics, a new Camera API, and the ability to auto-unlock your mobile phone if an authorized android Wear is near.
	If you have a Nexus 5 or a Nexus 7 (2013), you can install the Developer preview available here http://developer.android.com/preview/setup-sdk.html

Android Wear
	The Android Wear platform is used in 2 smartwatches available now: the LG G and the Samsung Gear Live.
	What kind of information can you see on Android Wear? More or less your Google Now cards and notifications. Voice control is also active.

Android TV
	Want to install Android apps on your TV (or set-top-box)? Want to broadcast your photos or videos from your phone to the big screen? Google got you covered.
	Android TV brings chromecast functionnality (Google cast) to set-top boxes or smart TVs. And developers can write specific applications for the big screen.
	Google won't build hardware directly, but Sony and Sharp have announced products for their 2015 lineup. Also, it is already in the next Bouygues Telecom set-top-box http://techcrunch.com/2014/06/26/with-project-miami-bouygues-telecom-partners-with-google-to-reinvent-its-innovation-strategy/ Go France, go!

Android Auto
	Want to install Android apps on car? Want to ... WAT? You want to play Candy Crush while driving? Fortunately, it won't be possible.
	Google wants to replace your embedded infotainment system with Android L. One paired with your phone, you'll have Google Maps on a big(gish) screen, voice commands (not to drive the car of course), notifications, Music, Google Now...
	You will be able to interact with your phone not only via the screen, but also with the integrated car buttons.
	Developers will be able to write apps for the car, but it will have to be approved by Google.

Android - recap (opinionated, flame on)
	Android L is about Android everywhere. And it mostly is about having the same experience wherever your are, on whatever screen you are looking at. It will be easy to look at your watch to see who just sent you an SMS, and dictate the answer (provided you want to answer in english - for now). It will be easy to show everyone in the room the photos and videos you took on your holidays: turn on your TV and control the slideshow with your phone. It will be easy to ask your car to give you directions to a new place, and because Google Maps is always up-to-date and knows where the traffic is, there will be no bump in the road.
	It will be seamless because Android L is *meant* to run everywhere.

ChromeOS
	some android apps are "ported" to chromebooks
	better integration with phone: calls, text messages, notifications. Unlock chromebook when your phone is near

Chromecast
	Chromecasts are cheap and will be able to do more.
	Your friends will be able to cast photos or videos without being on your home wifi (via ultrasonic sounds http://gigaom.com/2014/06/26/chromecast-will-use-ultrasonic-sounds-to-pair-your-tv-with-your-friends-phones/)
	As a nice addition, you will be able to choose your background photos.

Drive
http://googledrive.blogspot.com/2014/06/newdocssheetsslides.html
	Edit Office files natively (without collaboration features though) in android or with chrome (https://chrome.google.com/webstore/detail/office-editing-for-docs-s/gbkeegbaiigmenfmjfclcdgdpimamgkj)
	With the new apps Docs, Sheets and Slides coming, QuickOffice will be discontinued.

Cloud
http://googlecloudplatform.blogspot.com/2014/06/reimagining-developer-productivity-and-data-analytics-in-the-cloud-news-from-google-io.html
http://googledevelopers.blogspot.fr/2014/06/cloud-platform-at-google-io-new-big.html
	Dataflow is a kind of successor to MapReduce: it works with stream and/or batch
	Cloud Debugger, Cloud Trace, Cloud Monitoring: new tools to be able to easily debug your apps running in the cloud.
	Cloud Save: an API to simply save and retrieve information on the cloud from your app. 

Cardboard VR
Mockulus thrift, as coined by TechCrunch. Its a poor-man's Occulus Rift. Made of cardboard and 2 lenses, you put your phone in it and launch a specific app. You'll be able to "be" pegman in Street view : move your head around and the 3D-view of the photos will move also. Fantastic! I've ordered 5 of these for us to experience it. I'll tell you when I'll have them.
http://techcrunch.com/2014/06/25/hands-on-with-googles-incredibly-clever-cardboard-virtual-reality-headset/
http://www.frandroid.com/marques/google/225608_google-cardboard-casque-realite-virtuelle-en-carton
https://developers.google.com/cardboard/

Of note
	Google fit : a set of APIs to let apps and peripheral makers put their data in a centralized place. It's an Apple's HealthKit competitor.
	Google Now can switch between languages automatically. Make a voice-activated Google search on your phone(/watch/car/tv) in English then dictate an email in French without touching your configuration. Great!
	"OK google" works from any screen (http://www.droid-life.com/2014/06/25/google-search-3-5-14-adds-ok-google-hotword-everywhere-plus-audio-history/). Just like in the Moto X.

Didn't announce
http://readwrite.com/2014/06/25/google-io-2014-what-we-didnt-get
	Almost nothing on Glass, except that it will be kind of transformed as a Google Wear device
	Nothing on Nest, their big take on the smart home.
	Google+. Nothing new on the social-network-everyone-should-check-out-because-it-is-not-the-ghost-town-some-would-like-it-to-be (trust me).
