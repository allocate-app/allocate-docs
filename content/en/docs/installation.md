---
title: "Installation"
slug: "installation"
aliases:
- "/install"
---

## Packaging

FINISH LITTLE EXPLANATION HERE

- Building for iOS/MacOS will require you to have, at least, a free [developer account](https://developer.apple.com/support/compare-memberships/)
- If you have a paid [developer account](https://developer.apple.com/support/compare-memberships/), consider following [this iOS guide](https://docs.flutter.dev/deployment/ios) / [this MacOS guide](https://docs.flutter.dev/deployment/macos)
- *I do not have a paid developer account and cannot help with distribution/export. These instructions assume you have a free developer account*
### Android

- No extra packaging is required
- The apk will be located at: allocate/build/app/outputs/bundle/release/allocate.apk
- Proceed to [installing](#android-1)

### iOS

- Open the XCode project:
```zsh
open ios/Runner.xcworkspace
```
- If you do not have a code-signing identity, [create one](https://developer.apple.com/documentation/xcode/sharing-your-teams-signing-certificates).
- Ensure the certificate is active:
  - XCode > Preferences... > Accounts
  - If the certificate is not active, select *Manage Certificates...* and generate a new certificate

#### Signing

- Select Runner from the left navigator drawer
- Select the Runner Project and ensure the iOS Deployment target is set to iOS 12.0 or greater
- Select the Runner Target
  - In the General tab, ensure the minimum deployment is set to iOS 12.0 or greater
    - Verify the identity is as follows:
      - App Category: Productivity
      - Display Name: Allocate
      - bundle identifier: com.jordan.allocate

  - In the Signing & Capabilities tab, set "Automatically manage signing"
    - Your team should be your personal team
    - Verify the bundle identifier is com.jordan.allocate

#### Packaging the IPA

- From the Finder bar:
  - Product > Clean Build Folder...
  - Product > Build For > Running

- When the project has finished building:
  - Product > Show Build Folder in Finder

- From the Build folder:
  - Products > Release-iphoneos
  - copy Runner.app to another location on your Mac (eg. your home folder, Documents, etc.)

- Open a finder window and navigate to where you copied Runner.app
- Make a folder called "Payload" (case-sensitive)
- Place Runner.app within the newly created "Payload" folder
- Right-click "Payload" (or highlight and open the context menu) and compress it to a .zip
- Rename Payload.zip to Allocate.ipa

- Proceed to [installing](#ios-1)

### Linux

### Windows




### MacOS



## Installing

### Android

Installing on an Android device is fairly straightforward.

- Connect your Android device via USB

- Install via flutter *(developer mode may be required)*:
  - Open a command promt and navigate to the allocate folder and execute:
```bash
flutter install
```
- Install via sideloading:
  - Copy allocate.apk from allocate/build/app/outputs/bundle/release/ to your device.
  - Allow installing apps from untrusted developers in settings
  - Install the apk using your file manager

### iOS

*If your device is jailbroken, just install the ipa via your preferred method*

Installing on an iOS device *(without a paid account)* is more complicated. While it is possible to install your locally-signed .ipa to your device with Developer mode activated, the app will need to be resigned every 7 days. I would recommend using a sideloading alternative such as [AltStore](https://altstore.io/).

- [Install AltStore and AltServer](https://faq.altstore.io/) 
- Copy Allocate.ipa to your device (iCloud/AirDrop/etc.)
- Open AltStore on your iOS device:
  - My Apps tab > Tap the + > Navigate to Allocate.ipa in Files > Wait for AltStore to finish installing


### Linux

### Windows

### MacOS
