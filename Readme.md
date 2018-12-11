#Command Mobile Penetration Testing Cheatsheet

##ADB Cheatsheet

##Frida Cheatsheet
Install Frida Server on android,
download frida server : https://github.com/frida/frida/releases
```
$ adb root # might be required
$ adb push frida-server /data/local/tmp/
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server &"
```
