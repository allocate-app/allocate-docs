---
title: "Building Allocate"
slug: "building-allocate"
aliases:
- "/build"
---

## Prerequisites 

This guide assumes you have a basic understanding of the command line.

Allocate has been designed using Supabase for its online storage. If you would prefer an alternative, you will need to modify and build the project accordingly. These instructions only apply to using Supabase. For offline-only use, see [Building (Offline)](#offline).

MacOS and iOS build targets both require you to build using a Mac. If you do not have access to Apple hardware, consider using a cloud service like [codemagic](https://codemagic.io/start/).
  - Note: [codemagic](https://codemagic.io/start/) requires you to have a paid Apple developer account and assumes you will be deploying the app on the App store.
  - A paid Apple Developer ID is not required to build and install Allocate. See: [Installation]( {{< ref "installation.md" >}} )

### Build Tools 

  - [Flutter](https://docs.flutter.dev/get-started/install)
  - [Supabase CLI](https://supabase.com/docs/guides/cli/getting-started?queryGroups=platform&platform=npx) (optional: required for online)
  - (MacOS/iOS only) XCode
  - (Windows only) Visual Studio

### Database Hosting (Online only)

- [Supabase](https://supabase.com/)

## Dependencies

### Flutter
  - Install the [Flutter](https://docs.flutter.dev/get-started/install) sdk for your operating system as well as any other required development tools (eg. XCode, Android Studio) 
  - Ensure that flutter has been added to your PATH 
  - Verify using:

```shell 
flutter doctor -v
```

### Supabase
  - Set up a new [Supabase](https://supabase.com/) project and configure the Authentication as follows:
    - Set the Site URL to io.allocate://login (the slug doesn't matter, only the deeplink URI)
    - Configure the following Email templates: Confirm signup, Magic link, and Change Email Address
      - These templates must include the {{ .Token }}. Allocate uses a 6-digit OTP to confirm sign-in/up and email changes
    - Configure the Email Auth provider by setting the following:
      - Enable Email Provider
      - Confirm Email
      - Email OTP Length: 6
      - (Optional) Email OTP Expiration
  - Open the SQL Editor:
    - Copy the schema.sql from the supabase\_config folder into the editor and run the code

  - Keep your Supabase URL, Supabase Annon Key, and database password handy

### Supabase CLI

  - Install the [Supabase CLI](https://supabase.com/docs/guides/cli/getting-started?queryGroups=platform&platform=npx)
  - If you install using npm/bun, you will need to [build manually](#building-manually) or set up a shell alias 
  - Otherwise, feel free to use the build\_allocate script for your operating system


### Allocate Project

  - Clone or download [Allocate](https://github.com/jordan-clayton/allocate), (unzip if downloaded)

## Building 

  ### Online

  - Navigate to the allocate project folder
  - Open build\_allocate.sh (MacOS/Linux) or build\_allocate.cmd (Windows) in a basic text editor and complete the following fields:
    - supabase\_url="your-url-goes-here"
    - supabase\_annon\_key="your-key-goes-here"
    - target\_os="your-target-os" (macos/windows/apk/ios etc.)

  - [Build manually](#building-manually) or open a command line from the directory and run ./build.sh or (WINDOWS).\build.cmd
    - This script assumes your [Supabase CLI](#supabase-cli) can be run without a prefix (eg. npx/bunx)
    - Follow the prompts accordingly to link supabase
    - You will need to provide your database password


  ### Offline

  - [Install flutter](#flutter) 
  - [Download Allocate](#allocate-project)

  - Navigate to the allocate project folder
  - Open build\_allocate.sh (MacOS/Linux) or build\_allocate.cmd (Windows) in a basic text editor and complete the following fields:
    - offline\_only="true"
    - target\_os="your-target-os" (macos/windows/apk/ios etc.)

  - [Build manually](#building-manually) or open a command line from the directory and run ./build.sh or (WINDOWS).\build.cmd
    - You will need to rebuild the project with a [Supabase](#supabase) database if you want to be able to sync online

  ### Building manually

  Open a command prompt, navigate to the allocate directory and input the following commands:

  - Get dependencies:

  ```bash
  flutter pub clean
  flutter pub get
  ```
  - Regenerate build graph and generate asset files:

  ```bash
  dart run build_runner build # Enter 1 if prompted to re-generate files
  dart run icons_launcher:create
  dart run flutter_native_splash:create
  ```

  - If building offline only:
  ```bash
  flutter build "your-os-target-here" --release --dart-define=OFFLINE=true
  ```

  - Otherwise, build online:
  ```bash
  flutter build "your-os-target-here" --release --dart-define=SUPABASE_URL="your-supabase-url" --dart-define=SUPABASE_ANNON_KEY="your-supabase-key" 
  ```

  - Configure Supabase CLI __(required for online)__:

  ```bash
  supabase login
  supabase init
  supabase link
  ```

  - Set up the edge function __(MacOS/Linux):__

  ```bash
  cp -r supabase_config/functions supabase/
  ```
  - Set up the edge function __(Windows CMD):__

  ```cmd
  robocopy .\supabase_config\functions .\supabase\ /E 
  ```

  - (Alternatively, copy the functions folder to the local supabase folder using your file explorer)
  <br></br>

  - Deploy the edge function to Supabase:
  ```bash
  supabase functions deploy delete_user_account
  ```


## Finish

After successfully building Allocate, the application, build libraries, and data will be located in the build/your-os-target/ folder.
At this point, there are still a few steps required to [package and install]( {{< ref "installation.md" >}} ) Allocate.

