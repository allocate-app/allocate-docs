---
title: "Installation"
slug: "installation"
aliases:
- "/install"
---

## Packaging

Due to the required post-build configurations, it is not possible to automate the process of packaging Allocate for installation on your device.
This page contains instructions for how to do so for your desired target operating system. Depending on your target the packaging (and installation) process can be a little tedious, but it should be relatively straightforward.

A few notes for Apple operating systems:

- Packaging for iOS/MacOS will require you to have, at least, a free [developer account](https://developer.apple.com/support/compare-memberships/)
- If you have a paid [developer account](https://developer.apple.com/support/compare-memberships/), consider following [this iOS guide](https://docs.flutter.dev/deployment/ios) / [this MacOS guide](https://docs.flutter.dev/deployment/macos)
- *These MacOS/iOS instructions assume you have a free developer account*

A few notes for Linux:
- Given the myriad of available distributions, it is beyond the scope of this guide to cover packaging and installation for every distro
- This guide tries to be as agnostic as possible, but it mainly assumes you are running Debian or one of its derivatives
  - You may need to search for equivalent dependencies
  - If you dislike AppImages there are some suggested alternative guides to follow
  - You might need to seek out how to build a package for your package manager
- *It is not required* that you package and install Allocate, but it is generally more convenient
  - It is possible to copy the bundle to another location in your computer and run Allocate from the folder
  - There is a sample [.desktop file](#desktop-integration) you can use to run Allocate from your launcher

### Android

- No extra packaging is required
- The apk will be located at: allocate/build/app/outputs/bundle/release/allocate.apk
- Proceed to [installing](#android-1)

### iOS

- Open the XCode project:
  - Open terminal.app and navigate to the allocate folder
```shell
cd path-to-allocate
open ios/Runner.xcworkspace
```
- If you do not have a code-signing identity, [create one](https://developer.apple.com/documentation/xcode/sharing-your-teams-signing-certificates).
- Ensure the certificate is active:
  - From the Finder Bar:
    - XCode:  
      - Preferences...  
        - Accounts
  - If the certificate is not active, select *Manage Certificates...* and generate a new certificate

#### Build Settings

- Select Runner from the left navigator drawer
- Select the Runner Project and ensure the iOS Deployment target is set to iOS 12.0 or greater
- Select the Runner Target:
  - In the General tab, ensure the minimum deployment is set to iOS 12.0 or greater
    - Verify the identity is as follows:
      - App Category: Productivity
      - Display Name: Allocate
      - bundle identifier: com.jordan.allocate

#### Signing
  - In the Signing & Capabilities tab, set "Automatically manage signing"
    - Your team should be your personal team
    - Verify the bundle identifier is com.jordan.allocate

#### Packaging the IPA

- From the Finder bar:
  - Product:
    - Clean Build Folder...
  - Product:
    - Build For 
      - Running

- When the project has finished building:
  - (Finder Bar) Product: 
    - Show Build Folder in Finder

- From the Build folder:
  - Navigate to: 

```
Products/Release-iphoneos
```

  - Copy Runner.app to another location on your Mac (eg. your home folder, Documents, etc.)

- Open a finder window and navigate to where you copied Runner.app
- Make a folder called "Payload" (case-sensitive)
- Place Runner.app within the newly created "Payload" folder
- Right-click "Payload" (or highlight and open the context menu) and compress it to a .zip
- Rename Payload.zip to Allocate.ipa

- Proceed to [installing](#ios-1)


### MacOS
- Open the XCode project:
  - Open terminal.app and navigate to the allocate folder
```shell
cd path-to-allocate
open macos/Runner.xcworkspace
```

- If you do not have a code-signing identity, [create one](https://developer.apple.com/documentation/xcode/sharing-your-teams-signing-certificates).
- Ensure the certificate is active:
  - XCode: 
    - Preferences...
      - Accounts
  - If the certificate is not active, select *Manage Certificates...* and generate a new certificate

#### Build Settings
- Select Runner from the left navigator drawer
- Select the Runner Project:
  - Select the info tab, ensure the minimum MacOS target is set to MacOS 10.15 or greater
  - Select the Build Settings tab:
    - In Build Options, set User Script Sandboxing to NO

- Select the Runner target:
  - In the general tab, ensure the minimum MacOS target is set to MacOS 10.15 or greater
  - Verify the identity is as follows:
    - App Category: Productivity
    - Display Name: Allocate
    - bundle identifier: com.jordan.allocate

#### Signing
- In the Signing & Capabilities tab, set Signing (Release) to "Automatically manage signing"
  - Your team should be your personal team
  - Verify the bundle identifier is com.jordan.allocate
  - Set the Signing Certificate to Development

#### Packaging the AppBundle
- From the Finder bar:
  - Product:  
    - Scheme: 
      - Edit Scheme:
    - Select the Run tab and set the following:
      - Build Configuration: Release
      - Executable: Allocate.app
      - Turn off Debug executable
  - Product: 
    - Clean Build Folder...
  - Product:  
    - Build For  
      - Running
  - When the project has finished building:
    - Product: 
      - Show Build Folder in Finder
  
  - Allocate.app will be located in: 

```
  Products/Release/
```

- At this point, you could package Allocate.app into a .dmg using Disk Utility or [an alternative](https://www.npmjs.com/package/appdmg)
- This is not necessary for [installation](#macos-1) or distribution; you can freely send Allocate.app to whomever you wish

- Proceed to [installation](#macos-1)

### Windows

- Download and install [inno setup](https://jrsoftware.org/isdl.php#stable)
- The executable, data, and most required .dll files will be located at:
```
allocate\build\windows\runner\Release
```

- Follow this [guide](https://medium.com/@fluttergems/packaging-and-distributing-flutter-desktop-apps-the-missing-guide-part-2-windows-0b468d5e9e70):
  - Ignore step 2; the project has already been built
  - You do not need to add a license.txt
  - Licences are accessible via the [settings screen]({{< ref "user-settings.md" >}}#about) 
  - Continue up to step 6 to finish installing Allocate 

### Linux

- The Allocate binary will be located at:
```
allocate/build/linux/x64/release/bundle
```
- Depending on what system libraries you already have installed, you might require to install a few dependencies
- Determine what these are by running the following:

```shell
ldd build/linux/x64/release/bundle/allocate
```
 __or__
```shell
cd build/linux/x64/release/bundle/
ldd allocate
```

- If you are missing any of the required libraries, install them via your your package manager

#### Packaging
##### Linux-Dependencies
Allocate relies on the following external dependencies:


```
# These are for debian - they may be named differently in your distribution
- libgtk-3-0
- libayatana-appindicator3-1
- libc6-i386|libc6
- libcap2
- libgcrypt20
- libglib2.0-dev
- libjpeg-turbo8
- liblz4-1
- liblzma5
- libstdc++6
- libzstd1
```

- If you know how to package for your distribution, feel free to skip this section (and the [install](#linux-1) instructions)
  - Icon assets are all located in the assets folder for you to use as you see fit
  - Alternatively, [this guide](https://medium.com/@fluttergems/packaging-and-distributing-flutter-desktop-apps-the-missing-guide-part-3-linux-24ef8d30a5b4) will show you how to build .deb and .rpm builds using [flutter\_distributor](https://pub.dev/packages/flutter_distributor)
    - If you use [flutter\_distributor](https://pub.dev/packages/flutter_distributor) you will need to add the following: 
      - build-args to your distribute\_options.yaml
      - [dependencies](#linux-dependencies) to your make\_config.yaml
      - assets/allocatelogo.png for your application icon *(recommended)*

__Online build args:__

```yaml
build_args:
  dart-define:
    SUPABASE_URL: your-supabase-url-here
    SUPABASE_ANNON_KEY: your-annon-key
```
__Offline build args:__

```yaml
build_args:
  dart-define:
    OFFLINE: true
```

- Otherwise, these instructions will walk you through building an [AppImage](https://appimage.org/)
  - Note: The AppImage binary will be significantly larger than packages built for your distribution
  - This is due to the nature of AppImages, which include all shared library dependencies within the package
- Referring to [this guide](https://appimage-builder.readthedocs.io/en/latest/examples/flutter.html):
  - Install [appimage-builder](https://appimage-builder.readthedocs.io/en/latest/intro/install.html#appimage)
    - Using the AppImage is recommended
    - Moving the executable to a directory on your `$PATH` is also recommended to be able to execute via the command line

- Run the following commands *(from the allocate directory)*:

```shell
cp build/linux/x64/release/bundle AppDir
appimage-builder --generate
```

- Provide the following Basic Information:

```
Basic Information:
? ID [Eg: com.example.app] : com.jordan.allocate
? Application Name : Allocate
? Icon : Allocate
? Version : latest
? Executable path relative to AppDir [usr/bin/app] : allocate
? Arguments [Default: $@] : $@
? Update Information [Default: guess] : guess
? Architecture :  amd64
```

- Open the newly created AppImageBuilder.yml recipe and add the following:

```yaml
version: # This should be generated
script:
  - rm -rf AppDir || true
  - cp -r build/linux/x64/release/bundle AppDir
  - mkdir -p AppDir/usr/share/icons/hicolor/256x256/apps/
  - cp assets/allocatelogo256.png AppDir/usr/share/icons/hicolor/256x256/apps/Allocate.png
AppDir:
  path: ./AppDir
  app_info: # This should mostly be generated provided you included the Basic Information
    id: com.jordan.allocate 
    name: Allocate
    icon: Allocate
    verion: latest
    exec: allocate
    exec_args: @0
  
  apt:  # This should be your primary package manager, apt, pacman, etc.
    ...
    ... # This information should mostly be generated and not need modification
    ...
    include:   # These might be named differently depending on your distribution
      - libgtk-3-0
      - libayatana-appindicator3-1
      - libc6-i386|libc6
      - libcap2
      - libgcrypt20
      - libglib2.0-dev
      - libjpeg-turbo8
      - liblz4-1
      - liblzma5
      - libstdc++6
      - libzstd1
```

- Finally, run the following command:
```shell
appimage-builder --skip-test --recipe AppImageBuilder.yml 
```

*(Docker and a suite of images are required to run the test step)*

- In the allocate directory, you should find the following file:

```
Allocate-latest-<arch>.AppImage
``` 
*(\<arch\> will be whichever architecture target you built for, eg. x86_64/amd64/etc.)*

- Proceed to [installing](#linux-1)

## Installing

### Android

Installing on an Android device is fairly straightforward.

- Connect your Android device via USB

- Install via flutter *(developer mode may be required)*:
  - Open a command promt and navigate to the allocate folder and execute:
```shell
flutter install
```
- Install via sideloading:
  - Copy allocate.apk to your device from:

```
allocate/build/app/outputs/bundle/release/
``` 

  - Allow installing apps from untrusted developers in settings
  - Install the apk using your file manager

### iOS

*If your device is jailbroken, just install the ipa via your preferred method*

Installing on an iOS device *(without a paid account)* is more complicated. While it is possible to install your locally-signed .ipa to your device with Developer mode activated, the app will need to be resigned every 7 days. It is recommended to use a sideloading alternative such as [AltStore](https://altstore.io/).

- [Install AltStore and AltServer](https://faq.altstore.io/) 
- Copy Allocate.ipa to your device (iCloud/AirDrop/etc.)
- Open AltStore on your iOS device:
  - My Apps tab: 
    - Tap the + icon
    - Navigate to Allocate.ipa in Files 
    - Wait for AltStore to finish installing

- On first open, the app will request network access and permission to send notifications
- Network access is required for Allocate to have online functionality
- If you wish to receive notifications, allow them accordingly

### MacOS

- Copy Allocate.app from the [build folder](#packaging-the-appbundle) to your Applications folder
- Double click the icon to open Allocate:
  - MacOS will prevent you from opening the app on first open if you do not notarize the app
    - Notarizing requires a paid developer account
  - Dismiss the malicious software alert
- Right-click/Highlight and open the context menu, select open
  - Select open on the malicious software alert
  - This only needs to be done once; opening the app again will behave like any other MacOS application
- Allow access to the Documents folder (for the local database)
- If Allocate requests access to network, allow it to let Allocate connect to the internet
- Allow Allocate to send notifications if you wish to receive notifications 
  
### Linux
If you have bundled your application using a method other than [AppImage](https://appimage.org/), these instructions will not apply.

- Add execute permissions to the AppImage:

``` script
chmod a+x Allocate-latest-<arch>.AppImage
```

*(\<arch\> will be whichever architecture target you built for, eg. x86_64/amd64/etc.)*

- Copy the AppImage to wherever you wish, eg. $HOME/Applications/ or similar
  - You could also install to your /usr/local/bin/ and rename the package to something like "allocate" to run via the command line
- Allocate can then be run by double-clicking the icon, or right-click and select run
- (or execute via the command line: path/to/Allocate.AppImage)

### Desktop Integration
- To access Allocate via the launcher, you will need a .Desktop file
  - If you are unfamiliar with how to do this, [AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher) is a convenient tool

- Otherwise, feel free to modify and use the following:

__Allocate.desktop__

```toml
[Desktop Entry]
Type=Application
Version=1.0.0+6 # Set the version number here
Name=Allocate
GenericName=Task Manager
Icon=Allocate
Exec=path/to/Allocate.AppImage # Set the path to the executable here
Categories=Utility;
Keywords=Task management;Burnout;
StartupNotify=true
```

- And then run this command:

```shell
sudo cp Allocate.desktop /usr/share/applications/
```
- Allocate should then appear in your launcher
