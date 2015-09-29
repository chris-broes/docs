 {
  title: "Appium",
  description: "How to run Appium tests on Sauce Labs",
  category: 'Tutorials',
  index: 7,
  image: "/images/tutorials/appium.png",
}

Appium is an open-source tool that can be used to automate your mobile applications. Just like the Selenium WebDriver - which is an open-source tool used to automate web browsers - Appium is an automation library used to drive your mobile applications, and even the web browser within the mobile simulator, emulator or real device.

**Advantages of using Appium:**

- It allows you to write tests against multiple mobile platforms using the same API.
- You can write and run your tests using any language or test framework.
- It is an open-source tool that you can easily contribute to.

**Advantages of using Appium on Sauce Labs:**

- You save the time it takes to set up the Appium server locally.
- You don't have to install/configure the mobile emulators/simulators in your local environment.
- You don't have to make any modifications to the source code of your application.
- You can start scaling your tests instantly.

For an in-depth explanation of what Appium is, how it works and the various technologies used to make Appium work, visit the [Appium Documentation](http://appium.io/documentation.html). 

## Writing Tests With Appium

Getting started with Appium on Sauce Labs is really easy. As long as you are familiar with the fundamentals of automated testing, using [Selenium WebDriver](http://docs.seleniumhq.org/docs/03_webdriver.jsp) for automated tests, the specifics of your programming language and your test environment setup you should be able to follow along. All you need to do is the following:

1. Get a [Sauce Labs account](https://saucelabs.com/signup).
2. Decide on the type of mobile application that you will be testing:
   - [Mobile Native Application](#mobile-native-application) 
   - [Mobile Web Application](#mobile-web-application) 
   - [Mobile Hybrid Application](#mobile-hybrid-application) 
2. Write your mobile application tests using:
   - The [Appium](http://appium.io/slate/en/master/?python#appium-client-libraries) client library for the language of your choice.
3. Use the [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/) to obtain the  correct desired capabilities for your test.
4. Point the driver address to use the Sauce Labs cloud:
   - `http://SAUCE_USERNAME:SAUCE_ACCESS_KEY@ondemand.saucelabs.com:80/wd/hub`
5. Run your test and find the result in your [Sauce Labs test page](https://saucelabs.com/tests)

For more information on writing, configuring, and running your tests with Appium on Sauce Labs check out the [Running Your Mobile Test](#running-your-mobile-test) guidelines:

## Appium on Sauce Labs

### Running Your Mobile Test

During your mobile application automated tests, Appium will be the tool in the background that is starting and driving the simulator, emulator or device for each Sauce Labs test. For iOS **Appium is supported in iOS versions 6.1 and later**, for Android **Appium is supported in Android versions 4.4 or later for [Mobile Web Application](#mobile-web-application) tests and Android versions 2.3, 4.0 and later for [Mobile Native Application](#mobile-native-application) and [Mobile Hybrid Application](#mobile-hybrid-application) tests**.

First, decide on the type of mobile application that you will be testing. You can either test a [Mobile Native Application](#mobile-native-application) , a [Mobile Web Application](#mobile-web-application) or a [Mobile Hybrid Application](#mobile-hybrid-application) . If you are testing a [Mobile Native Application](#mobile-native-application) or a [Mobile Hybrid Application](#mobile-hybrid-application) , make sure that:

**For iOS**
- The mobile application is compiled in debug mode. 
- The mobile application is compiled for the simulator/device version of your choice.
- The mobile application is signed with a developer certificate (only if the application is signed). 
- The mobile application is hosted in a place that Sauce Labs can access, for example:
   - A remote location (e.g a GitHub Repository) 
   - Your [Temporary Sauce Storage](https://docs.saucelabs.com/reference/rest-api/#temporary-storage).

**For Android**
- The mobile application is compiled for the emulator/device version of your choice.
- The mobile application has [internet permissions](http://developer.android.com/reference/android/Manifest.permission.html#INTERNET). 
- The mobile application is hosted in a place that Sauce Labs can access, for example:
   - A remote location (e.g a GitHub Repository) 
   - Your [Temporary Sauce Storage](https://docs.saucelabs.com/reference/rest-api/#temporary-storage).   

Next, write your test using as a guideline the following Appium example tests:

**For iOS**
- [Java](https://github.com/appium/sample-code/blob/master/sample-code/examples/java/generic/src/test/java/SimpleIOSSauceTests.java)
- [PHP](https://github.com/appium/sample-code/blob/master/sample-code/examples/php/SauceTest.php)
- [Node.js](https://github.com/appium/sample-code/blob/master/sample-code/examples/node/ios-simple.js)
- [Python](https://github.com/appium/sample-code/blob/master/sample-code/examples/python/ios_sauce.py)
- [Ruby](https://github.com/appium/sample-code/blob/master/sample-code/examples/ruby/simple_test.rb)

**For Android**
- [Java](https://github.com/appium/sample-code/blob/master/sample-code/examples/java/junit/src/test/java/com/saucelabs/appium/AndroidTest.java)
- [PHP](https://github.com/appium/sample-code/blob/master/sample-code/examples/php/SauceTest.php)
- [Node.js](https://github.com/appium/sample-code/blob/master/sample-code/examples/node/android-simple.js)
- [Python](https://github.com/appium/sample-code/blob/master/sample-code/examples/python/android_simple.py)
- [Ruby](https://github.com/appium/sample-code/blob/master/sample-code/examples/ruby/android_on_sauce.rb)

The driver address in your test should be pointing to the Sauce Labs cloud (i.e `http://SAUCE_USERNAME:SAUCE_ACCESS_KEY@ondemand.saucelabs.com:80/wd/hub`), this is what indicates that the test will run on the Sauce Labs cloud. Also, for the corresponding programming language each test needs to extend/import the Appium client library instead of using the regular Selenium WebDriver client library. Finally, each test needs to be using the correct desired capabilities specifying the desired simulator/emulator platform, the Appium version and the mobile application location if using a mobile native app or a mobile hybrid app. 

For more information about the desired capabilities that you should be using during your mobile tests, please read the [Setting Desired Capabilities for Mobile Tests](#setting-desired-capabilities-for-mobile-tests) section.

Finally, once your test is ready to be executed use the test framework of your choice to run the test. As your test runs you should be able to spy on the test by going into your [Sauce Labs test page](https://saucelabs.com/tests). For more information about what you should look for when viewing the results of your test, please read the [Viewing My Appium Tests on Sauce Labs](#viewing-my-appium-tests-on-sauce-labs) section. 

To learn how Appium and Sauce Labs work read the [Understanding how Appium works on Sauce Labs](#understanding-how-appium-works-on-sauce-labs) section for an overview.

## Types of Mobile Applications

### Mobile Native Application 
This type of application is developed for an specific platform (i.e., iOS or Android), using the native SDKs provided by the platform vendor, and distributed to users via the appropriate app store.

### Mobile Web Application 
This type of application is formally what we call a mobile website. It can be accessed through a browser (e.g. Mobile Safari, Browser, Chrome, etc.) in a mobile simulator/emulator or real device. 

### Mobile Hybrid Application 
This type of application is part mobile native app and part mobile web app. Just like mobile native apps you can find and download mobile hybrid apps using the Apple’s App Store or the Google Play Store. In the same manner, just like a mobile web app, a mobile hybrid app would look like a mobile website that would be accessed through a "browser", but in this case the "browser" is an embedded webview within the application that would just allow to display some HTML. 

## Setting Desired Capabilities for Mobile Tests

The desired capabilities are a set of keys and values that will be sent to the Appium server running in the Sauce Labs cloud. These keys and values tell the Appium server the specifications of the automated test that you will be running. Using our [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/) you can easily determine the correct desired capabilities for the programming language of your choice.

Here is a list of the main desired capabilities that you will be using for your mobile tests:

- **browserName** (required): 
The mobile web browser that will be automated in the simulator, emulator or device (e.g Mobile Safari, Browser, Chrome, etc). If testing a mobile native application or a mobile hybrid application the value for this capability should be an empty string.

- **deviceName** (required):
The name of the simulator, emulator or device that will be used (e.g iPhone Simulator, iPad Simulator, Android Emulator, Samsung Galaxy S4 Emulator, Samsung Galaxy S5 Device, etc.). To specify Android phones and tablets read the [Android Emulator Skins](#android-emulator-skins) section.

- **platformName** (required):
The mobile operating system platform that will be used (e.g iOS or Android).

- **platformVersion** (required):
The mobile operating system version that will be used (e.g 8.0, 7.1, 4.0, 5.0, etc.).

- **appiumVersion** (optional):
The version of the Appium driver that will be used. It is recommended to specify the latest Appium version which is the one suggested by the [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/). If not specified the test will run against the default Appium version.

- **app** (only for mobile native application or mobile hybrid application tests):
The path to a .apk or .zip file containing the app to test. This could be the location of your app in the [Temporary Sauce Storage](https://docs.saucelabs.com/reference/rest-api/#temporary-storage) (e.g sauce-storage:myapp.zip) or the URL to a remote location where your app is located (e.g http://myappurl.zip). This capability is not required for Android if you specify the **appPackage** and **appActivity** capabilities.

- **device-orientation** (optional):
The orientation in which the simulator/device will be rendered (e.g portrait or landscape). 

- **appActivity** (optional): 
The activity name for the Android activity you want to launch from your package. This often needs to be preceded by a `.` (dot) (e.g., .MainActivity instead of MainActivity). Appium will automatically determine the activity to launch, you only need to use this desired capability if you want to specify an activity different than the default one.

- **appPackage** (optional): 
The Java package of the Android app you want to run (e.g com.example.android.myApp, com.android.settings, etc.). Appium will automatically determine the package to launch, you only need to use this desired capability if you want to specify a package different than the default one.

- **automationName**(optional): 
The automation engine that will be used (e.g Appium or Selendroid). By default the automationName used is Appium.

**Note 1**: For **Mobile Native Applications** tests using Android versions 2.3, 4.0 and 4.1 you need to specify the following desired capability: `"automationName":"selendroid"`. The reason for this is that these Android versions are only supported via the Appium’s bundled version of Selendroid, which utilizes [Instrumentation](http://developer.android.com/reference/android/app/Instrumentation.html). For the remaining versions of Android (i.e Android 4.2 and later) you do not need to use the `'automationName':'selendroid'` desired capability since these later versions are supported via Appium’s own UiAutomator library which is the default automation backend.

**Note 2**: For **Mobile Hybrid Applications** tests using Android versions 2.3 to 4.3 you need to specify the following desired capability: `"automationName'":"selendroid"`. For the remaining versions of Android (i.e Android 4.4 and later) you do not need to use the `'automationName':'selendroid'` desired capability since these later versions are supported via Appium’s own UiAutomator library which is the default automation backend.

For more desired capabilities read the [Appium Server Capabilities](http://appium.io/slate/en/master/?python#appium-server-capabilities) documentation.

### Android Emulator Skins

For your Android emulator test you can request a regular Android emulator by using the desired capability `"deviceName":"Android Emulator"`. But if instead you are interested in using an Android emulator that looks and feels like a certain Android phone or tablet (e.g Google Nexus 7 HD Emulator, Samsung Galaxy S4, Google Nexus 7C, etc) then instead of `"deviceName":"Android Emulator"` you need to specify the corresponding Android emulator skin (e.g `"deviceName":"Samsung Galaxy S4 Emulator"`).

Each Android emulator skins will have a different configuration depending on the phone or table that it is trying to emulate. For instance all the skins have different resolutions, screen dimensions, pixel densities, memory, etc. To get a list of the available Android emulator skins for the different Android emulator versions use our [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/).

## Viewing my Appium tests on Sauce Labs

Once your test is ready to be executed use the test framework of your choice to run the test. As your test runs you should be able to spy on the test by going into your [Sauce Labs test page](https://saucelabs.com/tests). In addition, after the test execution completes you will be able to see the commands executed during your test, the screenshots taken by Sauce Labs during your test, a video of of the test, the Appium server log and metadata information related to the test.

The **Appium Log** tab in your test indicates that this test ran using the Appium driver. If you take a look at the Appium Log, you will notice that the first line of the log provides information about the Appium version used during your test (e.g `info: Welcome to Appium v1.4.0`). 

For **iOS tests**, you will notice that the iOS simulator log is embedded within the Appium log. The information from the iOS simulator is grayed out throughout the Appium log and has the following tag name: `info: [IOS_SYSLOG_ROW ]`. For **Android tests** you can find the Android emulator logs by clicking on the "Metadata" tab in your test page and searching for the "Logcat.log" file. This file contains all the information from the Android emulator log.

## Understanding how Appium works on Sauce Labs

The driver address in you test script is what points the test to the Sauce Labs cloud (i.e `http://SAUCE_USERNAME:SAUCE_ACCESS_KEY@ondemand.saucelabs.com:80/wd/hub`). Once you execute the test using the test framework of your choice, Sauce Labs initializes its own Appium server using the specifications taken from your desired capabilities.

For **iOS tests**, after the mobile application is initialized, the Appium server takes the driver commands in your test script which are in a [WebDriver JSON Wire Protocol](https://code.google.com/p/selenium/wiki/JsonWireProtocol#Introduction) format and converts them into UIAutomation JavaScript commands which can be understood by [Apple Instruments](https://developer.apple.com/library/mac/documentation/DeveloperTools/Conceptual/InstrumentsUserGuide/Introduction/Introduction.html). These commands once converted are sent to your iOS mobile application via Apple Instruments. Apple Instruments is what executes the commands against your mobile application in the simulator/device.

The responses obtained from your mobile application are received by Apple Instruments, sent to UI Automation and relayed to the Appium server in the Sauce Labs cloud. The Appium server then needs to convert the UI Automation JavaScript responses back into WebDriver JSON Wire Protocol format and send the JSON responses back to your test script.

For **Android tests**, after the mobile application is initialized, the Appium server receives your test script commands and launches your mobile application in the emulator/device that you've specified. Appium then takes the driver commands in your test script which are in a [WebDriver JSON Wire Protocol](https://code.google.com/p/selenium/wiki/JsonWireProtocol#Introduction) format and converts them into UIAutomator Java commands. UIAutomator is the library provided by Google as part of the Android SDK and is the library that Appium uses to automate your Android mobile application tests.

In the case where you are connecting to the Selendroid automation backend, Appium simply proxies all requests to the Selendroid server running on the emulator/device.

The responses obtained from your mobile application are received by UIAutomator and relayed to the Appium server in the Sauce Labs cloud. The Appium server then needs to convert the UIAutomator Java responses back into WebDriver JSON Wire Protocol format and send the JSON responses back to your test script.

Appium hides all of this complexity from your test script (and from you!). Your test script thinks it's communicating with your mobile application, but in reality it is communicating with Appium's implementation of the WebDriver API. The fact that Appium is running your mobile application in the appropriate simulator, emulator or device and wrapping all of the communications with your mobile application including the commands conversions remains completely hidden, and your test script is none the wiser.

## Appium Resources

For more information, feel free to visit the resources listed below:

  - [Appium](http://appium.io)
  - [Introduction to Appium Concepts](http://appium.io/introduction.html)
  - [Appium Docs](http://appium.io/slate/en/v1.0.0/)
  - [Appium Discussion Group](https://discuss.appium.io)
  - [Appium on Github](http://github.com/appium/appium)
  - [Selenium WebDriver JSON Wire Protocol](http://code.google.com/p/selenium/wiki/JsonWireProtocol) 
  - [Selenium](http://www.seleniumhq.org)
  - [Apple UI Automation Documentation](http://developer.apple.com/library/ios/#documentation/DeveloperTools/Reference/UIAutomationRef/_index.html)
  - [Android UI Testing Documentation](http://developer.android.com/tools/testing/testing_ui.html)

## Frequently Asked Questions

- **How can I test Android tablets?**

The best way to test on different Android emulators screen sizes is by using the different [Android Emulator Skins](#android-emulator-skins) . For instance, if you use our [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/) you'll see the available skins for the different Android versions (e.g Google Nexus 7 HD, LG Nexus 4, Samsung Galaxy Nexus, Samsung Galaxy S3, etc). Some of these skins are tablets, for example the Google Nexus 7C is a tablet which has a very large resolution and very high density. 

- **How can I run manual tests for my mobile native app or mobile hybrid app?**

Sauce Labs doesn't support manual tests for mobile native app or mobile hybrid app tests.

- **What type of keyboard and buttons do the Android emulators have?**

Android Emulators have software buttons and a hardware keyboard. In a regular Android emulator the device buttons are software buttons displayed on the right size of the emulator. For the Android emulators with different skins (e.g Google Nexus 7 HD, LG Nexus 4, Samsung Galaxy Nexus, Samsung Galaxy S3, etc) the device buttons are also software buttons that are overlayed on top of the skin. For instance, if you hover the mouse around the edges of any of our Android emulators with an specified skin, a hover icon will appear and you should be able to find whatever buttons actually exist on the device that the skinned emulator is trying to emulate (e.g power button along the top, volume buttons along the edge, back/home buttons right below the screen, etc).

- **How can I run Android tests without Appium?**

For older versions of Android Appium might not be supported. For instance, Appium is only supported in Android versions 4.4 or later for [Mobile Web Application](#mobile-web-application) tests, and Android versions 2.3, 4.0 and later for [Mobile Native Application](#mobile-native-application) and [Mobile Hybrid Application](#mobile-hybrid-application) tests.

For those versions in which Appium is not supported you can request an emulator driven by Webdriver + Selendroid. All you need to do is use our [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/) and select **Selenium** for the API instead of Appium.

In the Sauce Labs test you will notice that the top of the emulator says "AndroidDriver Webview App". In addition, you will notice that you will get a "Selenium Log" tab which has the output of the Selendroid driver. 

With an emulator driven by Webdriver + Selendroid you will be able to test [Mobile Web Application](#mobile-web-application) only. You should be able to select any Android emulator version from 4.0 to the latest version and any Android emulator skin (e.g "deviceName":"Samsung Galaxy Tab 3 Emulator").

- **How can I run iOS tests without Appium?**

For older versions of iOS Appium might not be supported. For instance, Appium is supported in iOS versions 6.1 and later. For earlier versions of iOS the tool or driver used to drive your mobile applications automated test is called iWebdriver. 

To obtain a simulator driven by iWebdriver use our [Platforms Configurator](https://docs.saucelabs.com/reference/platforms-configurator/#/) and select **Selenium** for the API instead of Appium. With an emulator driven by iWebdriver you will be able to test [Mobile Web Application](#mobile-web-application) only. In addition, in the Sauce Labs test you will notice a "Selenium Log" tab which has the output of iWebdriver. 

- **What mobile web browsers can I automate in the Android emulator?**

Currently the only browser that can be automated in our Android emulators is the stock browser (i.e Browser). The Android stock browser is an Android flavor of 'chromium' which presumably implies that its behavior is closer to that of Google Chrome.

- **How can I test with mobile real devices instead of using the simulators or emulators?**

The mobile real device cloud is a new feature that Sauce Labs is currently working on. For more information about this feature please directly email one of our sales team representatives (saro@saucelabs.com).
<!-- should this be updated to reference our new state of the realdev? -->



