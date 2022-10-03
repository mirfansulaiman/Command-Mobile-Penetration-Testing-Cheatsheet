# Command Mobile Penetration Testing Cheatsheet
The things what you should know about android :) </br>
For iOS application please check [iOS Pentest Cheatsheet](https://github.com/mirfansulaiman/Command-Mobile-Penetration-Testing-Cheatsheet/blob/master/ios-cheatsheet.md).</br>

## ADB Cheatsheet
Download adb http://adbdriver.com/downloads/ or you can using adb as default from Android Studio.

### ADB Command
```
# Check Android Architecture
$ adb shell getprop | grep abi
# Try to use this command to get simple output :)
$ adb shell getprop ro.product.cpu.abi

# List all application already installed
$ adb shell pm list packages -f | grep -i 'testing'

# List All Processes on Android
$ adb shell ps
$ adb shell ps -A

# Stop Process on Android
$ adb shell kill [PID]

# Stop Package Process on Android
$ adb shell am force-stop <packagename>

# Tracing log on android
$ adb logcat | grep com.app.testing

# Install application to device
$ adb install app.testing.apk

# Get the full path of an application
$ adb shell pm path com.example.someapp

# Upload File to development machine
$ adb push frida-server /data/local/tmp/

# Download the apk to development machine
$ adb pull /data/app/com.example.someapp-2.apk

# Dump activity on app
$ adb shell dumpsys activity top | grep ACTIVITY

# Create new file in adb shell
$ cat > filename.xml
You can add lines to a text files using:
$ cat >> filename.xml
Both commands can be terminated using ctrl-D.

# Dump Memory
$ adb shell dumpsys meminfo com.package.name

# Disable verification adb
$ adb shell settings put global verifier_verify_adb_installs 0

# Disable verification package
$ adb shell settings put global package_verifier_enable 0
```

## Frida Cheatsheet
Install Frida Server on android,</br>
download frida server : https://github.com/frida/frida/releases
```
$ adb root # might be required
$ adb push frida-server /data/local/tmp/
$ adb shell "chmod 755 /data/local/tmp/frida-server"
$ adb shell "/data/local/tmp/frida-server &"
```

### Frida Command
```
# Connect Frida to an iPad over USB and list running processes
$ frida-ps -U

# List running applications
$ frida-ps -Ua

# List installed applications
$ frida-ps -Uai

# Connect Frida to the specific device
$ frida-ps -D 0216027d1d6d3a03

# Trace recv* and send* APIs in Safari
$ frida-trace -i "recv*" -i "send*" Safari

# Trace ObjC method calls in Safari
$ frida-trace -m "-[NSView drawRect:]" Safari

# Launch SnapChat on your iPhone and trace crypto API calls
$ frida-trace -U -f com.app.testing -I "libcommonCrypto*"

#Frida trace every open function while program start
$ frida-trace -U -i open com.app.testing

```

### Frida Tracing
Download : https://github.com/Piasy/FridaAndroidTracer
Usage :
```
$ java -jar FridaAndroidTracer.jar
-a,--expand-array      expand array values
-c,--classes <arg>     classes to be hooked
-j,--jars <arg>        jar files to be included
-o,--output <arg>      output script path
-p,--include-private   include private methods
-s,--skip <arg>        methods to be skipped
```

### Frida Trick
Bypass Root Detection:
Bypass anti-root detection in android application try to using different data type to break the logic flaws.

## Objection 
Install from https://github.com/sensepost/objection </br>
`pip3 install objection`</br></br>
Usage:</br>
Default Running Objection </br>
`objection --gadget "com.application.id" explore`</br></br>
Running Objection with command </br>
`objection --gadget "com.application.id" explore --startup-command "ios jailbreak disable"`</br></br>
Running Objection with script </br>
`objection --gadget "com.application.id" explore --startup-script antiroot.js`</br></br>
Inject Frida Gadget into APK with Objection</br>
`objection patchapk --source apkname.apk`</br>
After run application, the application will be paused and show the white screen at this moment you should run `objection explore` to resume the application.

## AndBug - For Enumerate Class And Method On Application
Download https://github.com/swdunlop/AndBug </br>
Usage:
```
#Enumerate classes on application
$ andbug classes -p [PID application / com.app.testing] > class.txt

#Enumerate methods on classes
$ andbug methods -p [PID application / com.app.testing] [class name]
```

## Android Log Tracing
Using PIDCAT : https://github.com/JakeWharton/pidcat </br>
Usage: 
```
$ ./pidcat.py [com.app.testing]
```

## Decompile APK File

### APKX for decompile apk
Download https://github.com/b-mueller/apkx </br>
Usage : 

```
$ apkx -c enjarify -d procyon app.testing.apk
```

### Bytecode Viewer - GUI
Download https://github.com/Konloch/bytecode-viewer/releases </br>
To read source code of dex or jar file. </br>
how to run : Just double click on jar file

### Reverse-Apk 
Download https://github.com/1N3/ReverseAPK </br>
Install :
```
$ git clone https://github.com/1N3/ReverseAPK.git
$ cd ReverseAPK 
$ ./install
```
Usage : 
```
$ reverse-apk app.testing.apk
```

## Install Burp Certificate On Android
Convert burp certificate from DER to PEM . If you lazy, you can download PEM file on this repository.
```
$ openssl x509 -inform DER -in cacert.der -out cacert.pem
# Get subject_hash_old (or subject_hash if OpenSSL < 1.0)
$ openssl x509 -inform PEM -subject_hash_old -in cacert.pem |head -1
$ mv cacert.pem 9a5ba575.0
```
Install PEM file to the System Trusted Credentials on device.
```
$ adb root
$ adb remount  
$ adb push 9a5ba575.0 /system/etc/security/cacerts/  
$ adb shell "chmod 644 /system/etc/security/cacerts/9a5ba575.0"
$ adb shell "reboot" 
```
If your /system cant mounting, You must mounting first.
```
$ adb root
$ adb shell
# Check mounting list
$ cat /proc/mounts
#/dev/block/bootdevice/by-name/system /system ext4 ro,seclabel,relatime,discard,data=ordered 0 0
$ mount -o rw,remount -t rfs /dev/block/bootdevice/by-name/system /system
$ adb push 9a5ba575.0 /system/etc/security/cacerts/  
$ adb shell "chmod 644 /system/etc/security/cacerts/9a5ba575.0"
$ adb shell "reboot" 
```

## Install Open Gapps On Android Emulator
Download : https://opengapps.org </br>
Extract :
```
$ unzip open_gapps-x86_64******.zip 'Core/*'
$ rm Core/setup*
$ lzip -d Core/*.lz
$ for f in $(ls Core/*.tar); do
  tar -x --strip-components 2 -f $f
done
```
Install to Emulator : 
```
$ adb root
$ adb remount
$ adb push etc /system
$ adb push framework /system
$ adb push app /system
$ adb push priv-app /system
$ adb shell stop
$ adb shell start
```

## Emulator
### Android Studio Emulator
This command for run emulator from android studio, make you have already install android studio before.</br>
if you want to root android emulator, please using system without (Google API's) or (Google Play) </br>
```
# List all emulator
$ emulator.exe -list-avds 
# Run Emulator
$ emulator.exe -avd [EmulatorName]
```

### Genymotion
Download https://www.genymotion.com/ 

### LDPlayer
Download https://www.ldplayer.net/

## QARK - Quick Android Review Kit
Download https://github.com/linkedin/qark </br>
For quick analyze application on android with scanning the apk or java file and create Proof Of Concept of vulnerability.</br>
Install QARK:
```
$ git clone https://github.com/linkedin/qark
$ cd qark
$ pip install -r requirements.txt
$ pip install . --user  # --user is only needed if not using a virtualenv
$ qark --help
```
Usage to scan APK:
```
$ qark --apk path/to/my.apk
```
Usage to scan Java source code files:
```
$ qark --java path/to/parent/java/folder
$ qark --java path/to/specific/java/file.java
```
# SCREEN MIRRORING ANDROID DEVICE TO LAPTOP OR COMPUTER
I believe you want to mirroring android screen to your laptop or computer, you can buy a software to do that or you can use this tool [SCRCPY](https://github.com/Genymobile/scrcpy) for free :D 
## Install SCRCPY
Mac : `brew install scrcpy`
Windows: https://github.com/Genymobile/scrcpy#windows 

## Usefull command
Run with window borderless : 
`scrcpy -t --window-title 'My Research' --always-on-top`

# Repackage / Modify APK

Step to repackaging apk.

1. Download original apk
2. Extrack apk with apktool

    `apktool d vantagepoint.apk -o vantagepoint`
3. Modify the apk 
4. Repackage apk with apktool

    `apktool b vantagepoint -o vantagepoint_bank_1.apk`
5. Align and Signing the APK with uber-apk-signer.
    - To create the certificate I used this tool [APK Signer Tool v2](https://shatter-box.com/download/apk-signer-tool-v2/), the tool has a GUI and these tools is capable to do align and singing the apk also.
    - Download : [Uber-Apk-Signer](https://github.com/patrickfav/uber-apk-signer)
    
    `java -jar uber-apk-signer-1.1.0.jar -a vantagepoint_bank.apk --ks vantagepoint.jks --ksAlias vantagepoint-pass --ksKeyPass 1234567 --ksPass 1234567 -o vantagepoint_bank_release`
    
6. Cek signed APK to verify the apk was sign with our certificate(optional)

    `keytool -list -printcert -jarfile "vantagepoint_bank-aligned-signed.apk"`

## For Lazy People :v

1. Automate Check Root Detection -> https://github.com/laconicwolf/Android-App-Testing/blob/master/check_for_root_detection.py
2. Automate Install Burp CA on Android -> https://github.com/laconicwolf/Android-App-Testing/blob/master/install_burp_cert.py
3. Automate Repackage Apk -> https://github.com/laconicwolf/Android-App-Testing/blob/master/repackage_apk_for_burp.py

## Contribution
if you have know about more command or a new trick to do something with Mobile Pentest, please let me know :) </br>
email : doctorgombal@gmail.com 
