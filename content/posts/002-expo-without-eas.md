---
title: 'How to build expo apps without EAS'
description: "After sdk 49 expo enables native folder, but it always require use of EAS to build then, here's how we can build .apk and .ipa without the need of EAS"
publishDate: "28 Mar 2024"
updatedDate: "28 Mar 2024"
tags: ["expo", "build"]
---

# How to build expo apps without EAS

Expo 49 comes with new workflows, with newer versions, expo has a prebuilt command which enables native folders, and also, comes EAS which is expo solution for building iOS and Android apps frictionless, but if, you want to use expo, but you already has a pipeline in the old way, or you just don’t want to rely in expo services, and build your own build server?

Here’s what this article came from, I will teach how you can build, android and iOS, everything from CLI, so. You can put in Jenkins, or whatever you want.

> I will consider you already have a expo app, with SDK 49 or higher, then let’s begin.

## Prebuild

The regular work flow with expo is using expo go, but if with need some native stuff, or just by actually building the app, you need to run `yarn expo prebuild` to have access to android and iOS folders, when you have those folder, you can run `yarn expo run:(android/ios)`, this is ok in development environment but when we talk in a build server, it’s a little different.

When you run prebuild, expo uses a template for creating the native folders, this template comes from NPM registry, but when you run this in a build server usually they don’t have access to NPM, you can take a look into expo docs, and it would say that you can pass a GitHub url to download your template, but this way can not work as expected sometimes, and you will always rely in a volatile thing such as a repo url

My tests was not working this way, so I had to look into prebuilt and get the real template that expo was using by default, this way I downloaded the template from NPM, and added into a tarball into our project, you can do this using curl, it would be something like this:

```
curl https://registry.npmjs.org/expo-template-bare-minimum/-/expo-template-bare-minimum-49.0.26.tgz --output template.tgz
```

And then you can use it in your prebuild command

```
yarn expo prebuilt —template template.tgz
```

Ok, this way we fixed the template used in our build server, now it can generate android and iOS folder, with no issue, let’s move on
## Android Builds

Android usually is easier than IOS, so let’s start with it.

With android folder, the regular way of build is using gradlew, so we can simply `./gradlew assembleRelease|assembleDebug` to generate the .apk, at a first thought, if you are used to native android development, you may think

> Oh, it’s just run assembleDebug for dev builds and assembleRelease for production builds

Well, this is not actually how expo works, I don’t know exactly how, maybe I was doing something wrong, but assembleDebug, just creates a expo dev client in a .apk, which if you try to deliver to QA or to other in your team, it will just ask for expo start, which is not useful, and release build will work just fine.

The fun thing, is that expo works with .env files, it took me a while to understand how they really work, but basically, .env.development is ready automatically by assembleDebug, and .env.produciton by assembleRelease, so my solution for this, may not be the best, but definitely works.

In my react native code, I was using env variables for using debug menu, and stuff like that, so my goal with builds is basically one dev build that enables debug menu, and a release build that hides it and we can deliver the app for our users, so, just for summarizing, here’s the whole idea:

- We have a .env file in root, that expo can read;
- AssembleRelease generate the .apk we want
- Merge this two info together, and then we have development and release builds

With that in mind, this is what we will have in a bash file

This for development builds.

```bash
cat ./env/.development > .env   
yarn prebuild:clean -p android   
cd android    
./gradlew app:assembleRelease
```

And this for production builds.

```bash
cat ./env/.production > .env   
yarn prebuild:clean -p android   
cd android    
./gradlew app:assembleRelease
```

Notice that the native code will always build release apk, but we are changing the content in .env file, this way the prebuild command will run considering those env variables, and when we build, the env will be all seated.

Important to do this env content change before the prebuild, otherwise the build can not read that fileAnd also, run a rebuild only for the OS you want, this way expo can makes things faster.And there we are, this way you have your .apk for testing and for production.

## IOS builds

IOS is not so different, the idea is pretty much the same, build only releases but with different env files, so the real, really REAL CHALLENGE HERE, is how to really make iOS build everything from CLI, I don’t want event to touch Xcode, the build server should do everything automatically.

I could explain all my steps here, but was basically try, test, wait the build run, get frustrated, and make it all again, by reading and changing, and a good idea, just a suggestion, is run `man xcodebuild`, I had to really read a bunch of parameters there to understand what could be done, this way I had this script for building

```bash
function build {

    IOS_DIR="."
    DERIVED_DATA=$IOS_DIR/DerivedData
    BUILD_DIR=$IOS_DIR/build
    IOS_OUTPUT_DIR=$IOS_DIR/output
    SDK="iphoneos"
    IOS_WORKSPACE="MyApp.xcworkspace"
    CONFIGURATION=Release
    DEVELOPMENT_TEAM="<YOUR DEVELOMENT TEAM ID>"
    SIGNER="<YOUR SIGNER>"

    echo "===Cleaning==="
    xcodebuild -quiet -derivedDataPath $DERIVED_DATA -scheme MyApp clean

    echo "==Building MyApp==="
    xcodebuild clean archive \
       -allowProvisioningUpdates \
       -allowProvisioningDeviceRegistration \
       -workspace $IOS_WORKSPACE \
       -scheme MyApp \
       -configuration Release \
       -derivedDataPath $DERIVED_DATA \
       ARCHS="arm64" ONLY_ACTIVE_ARCH=NO \
       PROVISIONING_PROFILE_SPECIFIER="" \
       OBJROOT="$BUILD_DIR" \
       DEVELOPMENT_TEAM="$DEVELOPMENT_TEAM" \
       -archivePath $BUILD_DIR/Release-iphoneos/MyApp.xcarchive \
       CODE_SIGN_STYLE=Automatic

    echo "Ipa MyApp"
    xcodebuild -exportArchive \
        -allowProvisioningUpdates \
        -archivePath $BUILD_DIR/Release-iphoneos/MyApp.xcarchive \
        -exportPath $IOS_OUTPUT_DIR \
        -exportOptionsPlist ../scripts/exportOptions.plist
}
```

And another one for changing the env content, for dev

```bash
cat ./env/.development > .env   
yarn prebuild:clean -p ios   
cd ios    
build
```

And for production

```bash
cat ./env/.production > .env   
yarn prebuild:clean -p ios   
cd ios    
build
```

With this, considering off course that you have everything set in your apple account, you may have the .ipa file generated.
And that’s all folks, it was way hard read and put all this content together, since expo docs always recommend EAS, but with focus we get there, hope I could help anyone else.
