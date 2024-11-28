# Command Mobile Penetration Testing Cheatsheet for iOS

**Caution** : The iOS application has a different environment than the Android application, some commands here only work with MacOS.

## Preparation
**Install Brew**, Open terminal (Finder -> Application -> Utilities -> Terminal) and run this command :</br>
`$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`</br></br>
**Install XCode**, Open terminal and run this command : </br>
`$ xcode-select --install`</br>
Or you can download manualy on [Apple Website](https://developer.apple.com/downloads)</br></br>

Regist your Apple ID as [Apple Developer Account](http://developer.apple.com/account). You don't need to pay as Apple Developer Program on this section just register your account as a developer, because we just need to get a Certificate for signing the ipa file on XCode or other tools.

## SSH Over USB (iPROXY)
http://iphonedevwiki.net/index.php/SSH_Over_USB

## Save SHSH blobs
This tool is needed to back up your SHSH blobs if you want to downgrade iOS.
### Blobsaver
[Blobsaver](https://github.com/airsquared/blobsaver)

# iOS Terminal Command 
```
# list all process
$ ps aux

# Kill application/process
$ killall [name]
```

## Download .ipa file from app store or iPhone/iPad device

### Need Jailbroken 
#### Clutch
[Clutch](https://github.com/KJCracks/Clutch)

#### Frida Script
[Frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)
command:
`$ iproxy 2222 22`
`$ ./dump.py BundleID`

## iOS Binary Analysis
### ios-Analysis
Download : [ios-analysis](https://github.com/IAIK/ios-analysis)

Install : 
```
$ git clone https://github.com/IAIK/ios-analysis
$ cd ios-analysis
$ git submodule update --init --recursive
```

if you get error like this :
`error: RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 60`

run this command:
`git config http.postBuffer 524288000`

## Sign & Install .ipa using windows

- Download [altsigner](http://qd.appdown.info/qd/altsignerqd/altsignerInstaller_1.1.0.0/AltSigner_Installer_1.1.0.0.exe)
- Install latest iTunes (note: install using the binary, do not install from microsoft store)
- Open Itunes and select devices to copy UDID of your devices
- Open altsigner.exe, fill your [email,password,UDID,.ipa file path]
![https://kubadownload.com/site/assets/files/2957/altsigner-ios-13.815x0-is.webp](https://kubadownload.com/site/assets/files/2957/altsigner-ios-13.815x0-is.webp)
- click sign to sign the `.ipa` files, click `install` to install the `.ipa`

## Sign & Install .ipa using online apps

- https://www.iosappsigner.com/
- https://www.installonair.com/
- https://www.diawi.com/

## Command line tools helper for iOS (Windows/Linux)

- Download imobileconfig for (windows/linux) from: http://docs.quamotion.mobi/docs/download/

## Running idevicedebug.exe on windows

Need this file `DeveloperDiskImage.dmg` & `DeveloperDiskImage.dmg.signature` 
(From xcode folder install on mac `/XCode/Contents/Developer/Platforms/IPhoneOS.platform/DeviceSupport/<iOS_version_number>/`)

- mount the developerdiskimage: ./ideviceimagemounter.exe "./DeveloperDiskImage.dmg"
- debug the apps : ./idevicedebug.exe -d run vantagepoint.id.Hook2

## Signing IPA File with Our Provisioning Profile

This tool is very helpfull :) [iOS App Signer](https://dantheman827.github.io/ios-app-signer/) is GUI based. 
To generate our provisioning profile you can do it on XCODE while install the application.

### Troubleshoot
- if you get error message like this ``` If you have previously trusted your certificate using Keychain, please set the Trust setting back to the system default ``` 
- Don't Panic ! 
- Do this step: 
 1. Go into Xcode and delete your account from Preferences
 2. Go to ~/Library/MobileDevice/Provisioning Profiles in finder and delete the files within
 3. Go into keychain and delete any personal certificates mentioning Mac Developer, iOS Developer, etc
 4. Add your account back into Xcode and choose to revoke the existing certificate (If you can't revoke, leave it)
 5. Go to xcode, and try to install dummy application into device. This step will trigger apple to generate new our certificate.
 6. And then, Open iOS App Signer
- Reference: https://forum.kodi.tv/showthread.php?tid=287814

# Screenshare from iphone Real Devices

## QuickTime Player: Allows you to view and record your iPhone’s screen over USB.

Steps:
1. Connect your iPhone to your Mac using a USB cable.
2. Open QuickTime Player on your Mac.
3. Choose File > New Movie Recording.
4. Click the small arrow next to the red record button in the QuickTime window and select your iPhone under the camera options.
5. The screen of your iPhone will appear on your Mac screen. You can start recording by pressing the Record button in QuickTime.

QuickTime allows you to record the screen of your real iOS device without requiring additional software or jailbreaks.

# Capture Log Specific iOS Application

## Idevicesyslog
Command:
```
idevicesyslog -m "Vantage"
```
-m option is only print messages that contain STRING

# xCode iOS Simulator Cheatsheet

## Xcode and simulator side by side
```
defaults write com.apple.iphonesimulator AllowFullscreenMode -bool YES
```
![https://miro.medium.com/max/5760/1*TG6MNh3_qCtGA8UNMjk7mQ.png](https://miro.medium.com/max/5760/1*TG6MNh3_qCtGA8UNMjk7mQ.png)

## List all simulators created
```
xcrun simctl list --json
```

## Launch simulator app 
```
open /Applications/Xcode.app/Contents/Developer/Applications/Simulator.app/
```

### Launch multiple simulators
```
xcrun simctl boot 12F3C6FB-1A8A-4D20-922F-2DB485F58F0F
xcrun simctl boot BE53CBFF-4900-4F10-A1D4-B451AB4C9E7E
open /Applications/Xcode.app/Contents/Developer/Applications/Simulator.app/
```

## Record simulator video
```
xcrun simctl io booted recordVideo — type=mp4 ./test.mp4
```

## Install .IPA file to Simulator
```
xcrun simctl install booted /path/to/your/appfile.app
```

### How to get .app file? 
1. Download the `.ipa` file
2. Ensure `.ipa` file has been signed
3. Rename the `.ipa` file to `.zip`
4. Extract .zip file
5. the .app file is inside folder `Payload`

## Find the app container
```
xcrun simctl get_app_container booted com.bundle.identifier
```

## Stream simulator logs
```
`xcrun simctl spawn booted log stream > test.log&`; open test.log;
```

## Filter Logs
```
xcrun simctl spawn booted log stream --predicate 'eventMessage contains "com.itkan.awesome"'
```

The below command will help you know fields for creating the needed predicate
```
xcrun simctl spawn booted log stream --style=json
```

### Credit
[Ankit Kumar Gupta](https://medium.com/@ankitkumargupta/ios-simulator-command-line-tricks-ee58054d30f4)

# Troubleshoot !

## Xcode16: This device has reached the maximum number of installed apps using a free developer profile
1. Change with other apple account.
2. or Remove 1 apps :( in devices
 - Open Xcode
 - Open "Devices and Simulator" Using shortcut `command + shift + 2`
 - Remove 1 apps
![Screenshot 2024-11-28 at 11 32 36](https://github.com/user-attachments/assets/54722d34-37d9-47b4-a250-55c81b229a06)
 - and Try again install the apps !


## Xcode16: Could not locate device support files
Solution: https://github.com/iGhibli/iOS-DeviceSupport 

