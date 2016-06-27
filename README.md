# Cordova Hello World Plugin
#Taken from
https://github.com/don/cordova-plugin-hello


Simple plugin that returns your string prefixed with hello.

Greeting a user with "Hello, world" is something that could be done in JavaScript. This plugin provides a simple example demonstrating how Cordova plugins work.

## Using
Clone the plugin

    $ git clone https://github.com/don/cordova-plugin-hello.git

Create a new Cordova Project

    $ cordova create hello com.example.helloapp Hello
    
Install the plugin

    $ cd hello
    $ cordova plugin add ../cordova-plugin-hello
    

Edit `www/js/index.js` and add the following code inside `onDeviceReady`

```js
    var success = function(message) {
        alert(message);
    }

    var failure = function() {
        alert("Error calling Hello Plugin");
    }

    hello.greet("World", success, failure);
```

Install iOS or Android platform

    cordova platform add ios
    cordova platform add android
    
Run the code

    cordova run 

## More Info

For more information on setting up Cordova see [the documentation](http://cordova.apache.org/docs/en/4.0.0/guide_cli_index.md.html#The%20Command-Line%20Interface)

For more info on plugins see the [Plugin Development Guide](http://cordova.apache.org/docs/en/4.0.0/guide_hybrid_plugins_index.md.html#Plugin%20Development%20Guide)

##My comments

Cordova Android Plugins are based on Android Webview.

Plugin consists of:

* Java class that extends CordovaPlugin class, and overrides execute method
* Javascript interface with cordova.exec method. It sends request to Webview, calling 
action (greet) method on the service (Hello.java) class

```js
exec(<successFunction>, <failFunction>, <service>, <action>, [<args>]);
```

* plugin.xml with injected feature element to match service element in javascript

```xml
<feature name="<service_name>">
    <param name="android-package" value="<full_name_including_namespace>" />
</feature>
```

##Working in Threads 
###Access to UI:

use the Activity's runOnUiThread method

```java
cordova.getActivity().runOnUiThread(new Runnable() {
	public void run() {
	    //...
	        callbackContext.success(); // Thread-safe.
	}
});
```

###No UI Access:

use ExecutorService obtained with cordova.getThreadPool()

```java
cordova.getThreadPool().execute(new Runnable() {
	public void run() {
	    //...
	    callbackContext.success(); // Thread-safe.
    }
});
```