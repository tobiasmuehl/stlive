
### "Sencha Touch Live" - Instantly sync your source code changes to multiple mobile devices.  

## Description

Traditionally when developing Sencha Touch apps on mobile and tablet devices you need to recompile the Sencha source code, then recompile that with the PhoneGap framework for each platform SDK and then redeploy each native app to the devices and emulators for testing (For Sencha apps this is done by `sencha app build -run native`). For native app development this can be a slow process that you have to repeat for each code change before you can test the change on a native device or emulator.

This tool allows you to massively speed up development of your PhoneGap and Sencha Touch native apps by **skipping** all of these steps!

Using this tool you can update any Javascript, CSS or HTML source file on your development computer and it will instantly load your changes and restart the app on your device or emulators. This means you can live edit and test that your code change works on multiple devices! It even preserves the current client side route so in most cases you can immediately retest the active view without having to re-navigate to that view.

This means you can place any number of devices/emulators in front of you and instantly see the effect of your last code change one or more Android, iOS and WP8 devices!  You can even serve up source code from your local computer onto a cloud based device farm to instantly see your change on hundreds of different mobile device types.  

You can also keep your remote debuggers connected while each update occurs so you save more time by not having to restart your remote debuggers.  Since the Javascipt source code is not minified, it's also much easier to debug. You can also elect to load the original Sench Touch JS class files onto the device making debugging of framework code easier at some small cost to app reload time.

### The Problem

The PhoneGap team recently released [PhoneGap Developer App](http://app.phonegap.com/) that supports live updating of source code in a PhoneGap 3.x project. The current PhoneGap Developer App is available as a download from the app stores. It's great for trying out PhoneGap with a standard set of core plugins but unfortunately it is unusable for many production projects as you're locked into their fixed set of PhoneGap plugins that often don't match types of plugins or plugin versions or internal customisations required by your app. You also can't use any internally developed plugins.

What you really need is a live update client and server that have identical plugins to your final mobile app. For Sencha Touch development you also need additional features and optimisations not supported by the current PhoneGap App project.

### The Solution

To solve this, I created **stlive** to create, modify and serve live editable Sencha Touch or PhoneGap projects.

This tool allows you to instrument a new or existing mobile project to support live updating. On startup it offers you the options to either (a) run your existing native app or (b) start up a live update client that connects to a server that dispatches your original unmodified HTML/CCS/JS source code from your project folder onto the device.  It also dispatches platform specific code (e.g. Cordova plugin javascript) from your project. The server then watches for any changes in your source code files and will notify the client to reload the project code and restart your app whenever you change a source file.

Unlike PhoneGap Developer App, the live update client and server components and your final native app are now running identically configured Cordova plugins since they are all using the same PhoneGap project instance to do so.  For testing purposes you can be assured that the native app and live update client will be running identical PhoneGap configurations since they are now compiled and deployed as one app and the server is dispatching the same plugin source code.

Using this tool you should be able to complete most of your development and testing using the live update client, only needing to rebuild and redeploy when your project's Cordova plugin configuration is changed.

This tool is based on the same open source technology as the PhoneGap App but it's been specifically optimised for use with the Sencha Touch framework.

## Installation Requirements

To use the Sencha Touch features of this app you must first prepare a Sencha Touch and PhoneGap development environment by following the **System Setup** as outlined on the [Sench Touch Guide](http://docs.sencha.com/touch/2.3.1/#!/guide/command).

This app has been tested with:
 
- [Sencha Cmd 4.0.4.84](http://www.sencha.com/products/sencha-cmd/download) 
- [Sencha Touch 2.3.2](http://www.sencha.com/products/touch/download/) 
- [PhoneGap 3.4.0](http://phonegap.com/install/)

### Command Summary

  - **stlive create**   - Creates a new Sencha Touch 2.x + PhoneGap 3.x project with an embedded live client.
  - **stlive live add** - Add a live client to an existing Sencha Touch or PhoneGap project.
  - **stlive live remove** - Removes the live client from a project (for app store or production MDM deployment).
  - **stlive serve**    - Runs a live update server in your Sencha Touch or PhoneGap project folder.

  - **stlive version**  - Displays app version
  - **stlive settings** - Displays configured settings.  You can update these in `~/.stlive.config`

All default settings can be overridden using corresponding command line options.

## Installation

    $ npm install stlive -g

## Example

1. Create and compile a new Sencha Touch / PhoneGap app:

    $ stlive create -n DemoApp -d

2. Deploy the compiled APK file to an Android device or emulator:

    `DemoApp/phonegap/platforms/android/ant-build/DemoApp-debug.apk`

3. Run the live update server from your project folder.  
   - The server should then display the **IP Address** and **Port number** it's listened on.

    $ cd DemoApp
    $ stlive serve
    listening on 192.168.0.17:3000

4. Start the app on your device and select the Live Update link then key in the **IP Address** and **Port number** to connect to the server.

  - You should see the server display the sources files the client app is requesting.
  - For Cordova platform files, it will also display the actual file path dispatched (in green).  
  - You can use this to identify and fix any network or project configuration issues.
  - Finally you should see your new Sencha Touch displayed on your device or emulator.

5. Now edit the view that is displayed:
   - Open `app/views/Main.js` and change the Welcome message and save the file.
   - You should see the server reload the app and the new Welcome message displayed on the device.

### Configuration Options

The first time you run this app it will create a **~/.stlive.config** file that defines common properties and plugins it will preconfigure in new apps.  You can edit this file or use corresponding command line options to override these defaults. The properties are all documented with comments inside this file. You'll find it helpful in speeding up creating new apps and ensuring they are consistently configured. For example, you can preconfigure defaults like your company's reversed domain name (com.mycompany) or a set of commonly used plugins.

### Known Issues

- Navigating back to the start page and then re-selecting the Live Update link sometimes appears to fail to restart the client as expected.  You may then need to stop and restart the app.

### Licence

- Apache 2.0 

### Acknowledgements

- **Sench Touch** is a registered trademark of Sencha Inc.
- **Apache PhoneGap** is registered trademark of Adobe Systems Inc.
