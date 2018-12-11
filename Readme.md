# Command Mobile Penetration Testing Cheatsheet

## ADB Cheatsheet

## Frida Cheatsheet
Install Frida Server on android,</br>
download frida server : https://github.com/frida/frida/releases
```
$ adb root # might be required
$ adb push frida-server /data/local/tmp/
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server &"
```

### AndBug - For Enumerate Class And Method On Application
Download https://github.com/swdunlop/AndBug </br>
Usage:
```
#Enumerate classes on application
andbug classes -p [PID application / name of application] > class.txt

#Enumerate methods on classes
andbug methods -p [PID application / name of application] [class name]
```

## Install Burp Certificate On Android

## Android Log Tracing
Using PIDCAT : https://github.com/JakeWharton/pidcat </br>
Usage: 
```
./pidcat id.co.applikasi
```
