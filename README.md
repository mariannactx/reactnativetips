# REACT NATIVE TIPS for command line

## Troubleshooting:

When running npm, you may see an error that says something like "ENOENT". This is a problem with the number of watches your computer can handle.
You can increase this number by doing this:

```shell
    sudo echo 256 | sudo tee -a /proc/sys/fs/inotify/max_user_instances
    sudo echo 32768 | sudo tee -a /proc/sys/fs/inotify/max_queued_events
    sudo echo 65536 | sudo tee -a /proc/sys/fs/inotify/max_user_watches
```

## Daily command lines
When using a phone for testing

Go to the tools folder
```shell
cd /<path-to-android-sdk-folder>/Android/Sdk/platform-tools
```

To check if the phone is connected:
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

If you generated an apk, you also can install it by command line (instead of downloading the apk on the phone)
```shell
./adb install <path-to-apk>
```


