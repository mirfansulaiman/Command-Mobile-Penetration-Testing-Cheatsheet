# Command Mobile Penetration Testing Cheatsheet for iOS

**Caution** : iOS application has a different environment with Android application, some command in here is only working with MacOS.

## Preparation
**Install Brew**, Open terminal (Finder -> Application -> Utilities -> Terminal) and run this command :</br>
`$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`</br></br>
**Install XCode**, Open terminal and run this command : </br>
`$ xcode-select --install`</br>
Or you can download manualy on [Apple Website](https://developer.apple.com/downloads)</br></br>

Regist your apple ID as [Apple Developer Account](http://developer.apple.com/account). You dont need to pay as Apple Developer Program on this section just register your account into developer, because we just need to get Certificate for signing ipa file on XCode or other tools.

## SSH Over USB (iPROXY)
http://iphonedevwiki.net/index.php/SSH_Over_USB

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
