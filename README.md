# REACT NATIVE TIPS for command line (w/ Android)

## 1. GETTING STARTED
Follow the instructions @[Getting Started](https://facebook.github.io/react-native/docs/getting-started.html)

ATTENTION: Building your app using Android Studio is not avoidable! Prepare your bottle of water and your favorite podcast.

### WATCHMAN INSTALLATION on Ubuntu

Before running the instructions, install the dependencies (I modified the install command from [this tutorial](https://medium.com/@vonchristian/how-to-setup-watchman-on-ubuntu-16-04-53196cc0227c))
```shell
sudo apt-get install -y libtool autoconf automake build-essential python-dev pkg-config
```

Then, go back to the Getting Started tutorial mentioned above

You will see something like:
```shell
Your build configuration:
    CC = gcc
    CPPFLAGS =  -D_REENTRANT -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE
    CFLAGS = -g -O2 -Wall -Wextra -Wdeclaration-after-statement -g -gdwarf-2 -fno-omit-frame-pointer
    CXX = g++
    CXXFLAGS = -g -O2 -Wall -Wextra -g -gdwarf-2 -fno-omit-frame-pointer
    LDFLAGS = 
    prefix: /usr/local
    version: 4.9.0
    state directory: /path/to/watchman
```

## 2. Troubleshooting:

### - When running npm, you may see an error that says something like "ENOENT". This is a problem with the number of watches your computer can handle.
You can increase this number by doing this:

```shell
    sudo echo 256 | sudo tee -a /proc/sys/fs/inotify/max_user_instances
    sudo echo 32768 | sudo tee -a /proc/sys/fs/inotify/max_queued_events
    sudo echo 65536 | sudo tee -a /proc/sys/fs/inotify/max_user_watches
```

### - When testing with *Jest*, you may see this error:
```shell
Couldn\'t find preset "module:metro-react-native-babel-preset" relative to directory "<path-to-your-project>"
```

Substitute this line on your .babelrc:
```
"presets": ["module:metro-react-native-babel-preset"]
```
To:
```
"presets": ["react-native"]
```

### - When running *react-native run-android* you may see this errors:
```
> SDK location not found. Define location with sdk.dir in the local.properties file or with an ANDROID_HOME environment variable.
 ```
 ```
> The SDK directory 'some/path' does not exist.
```

For the above erros, make sure the path to the Android home is set (see the Getting Started instructions)

You may also see this error:
```
> Could not find tools.jar. Please check that /usr/lib/jvm/java-8-openjdk-amd64 contains a valid JDK installation.
```

Change the version of java (in my case 8 -> 11) on your /etc/enviroment 

Run the new configuration

``` shell
    source /etc/enviroment
```

### - Renaming the app
Follow the instructions @[this tutorial](https://medium.com/the-react-native-log/how-to-rename-a-react-native-app-dafd92161c35). Also, change the "name" attribute on your package.json file

## 3. DAILY COMMANDS

Go to the tools folder
```shell
cd /<path-to-android-sdk-folder>/Android/Sdk/platform-tools
```

Check if the phone is connected or an emulator is initiated:
```shell
./adb devices
```

You will see something like this:
```shell
List of devices attached
* daemon not running; starting now at tcp:1234
* daemon started successfully
ABCDEFG12345678	unauthorized
```

### Testing with a phone:
In case you forgot to give the permissions for accessing the phone (so it says "unauthorized"), you can kill the server...
```shell
./adb kill-server
```

... and start again. Remember to authorize this time! Sometimes the phone asks twice for authorization
```shell
./adb devices
```

Now, you will see something like this. "Device" means you can use it:
```shell
List of devices attached
ABCDEFG12345678	device
```

When trying to run the app or debugging, your computer may not find your phone. Get the phone identifier by running the previous command
and do this:
```shell
./adb -s ABCDEFG12345678 reverse tcp:8081 tcp:8081
```

Now you can run the app on your connected device (use this time to get some water for you and your plants)
```shell
react-native run-android
```

To access the "more options", specially for enabling debug:
```shell
./adb shell input keyevent 82
```

To refresh the app:
```shell
./adb shell input text "RR"  
```

To see the phone screen on your computer (has some delay, test it before presenting!):
```shell
./adb shell screenrecord --output-format=h264 - | ffplay -
```

If you generated an apk, you also can install it by command line (instead of downloading the apk on the device)
```shell
./adb install <path-to-apk>
```

### Bonus: executable files (for Ubuntu)
See /executables