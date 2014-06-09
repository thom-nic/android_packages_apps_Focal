# Focal Camera for Android

This is the Focal camera with some enhancements.

## Build Instructions

This assumes Ubuntu x64 either native or in a VM.  Note if you're in a VM you'll need >30 GB of HDD.  Most of the instructions can be found here: 
[Cyanogen build for Grouper](http://wiki.cyanogenmod.org/w/Build_for_grouper#Prepare_the_Build_Environment)


Follow any build instructions e.g. [for Cyanogenmod](http://wiki.cyanogenmod.org/w/Build_for_grouper#Prepare_the_Build_Environment) 

```
sudo apt-get install -y bison build-essential curl flex git gnupg gperf libesd0-dev libncurses5-dev libsdl1.2-dev libwxgtk2.8-dev libxml2 libxml2-utils lzop openjdk-6-jdk openjdk-6-jre phablet-tools pngcrush schedtool squashfs-tools xsltproc zip zlib1g-dev

sudo apt-get install -y g++-multilib gcc-multilib lib32ncurses5-dev lib32readline-gplv2-dev lib32z1-dev ia32-libs
```

Make a directory, e.g. `~/android`  We'll call this `$ANDROID_HOME`

Download the Android SDK, choose 'SDK only' (not ADT.)  Unpack it inside `$ANDROID_HOME`.  Now you'll have a directory called `android-sdk-linux`.  Run `android-sdk-linux/tools/android` and install the latest version of the "SDK Tools," "SDK Platform-tools" and "SDK Build-tools."

Add the following to your .profile:
```bash
# for 64-bit host OS
export BUILD_HOST_32bit=1
export PATH=$PATH:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools
```

Run the following:
```
repo init -u git://github.com/CyanogenMod/android.git -b cm-11.0
```

Edit `.repo/manifest.xml` and add the following lines:

```xml
  <project path="packages/apps/Focal" name="xplodwild/android_packages_apps_Focal" revision="master" />
  <project path="packages/external/Focal" name="xplodwild/android_external_Focal" revsision="master" />
```

Then run:
```bash
repo sync # this takes a *looong* time
cd packages/apps/
git clone https://github.com/xplodwild/android_packages_apps_Focal.git Focal
cd ../../external/
git clone https://github.com/xplodwild/android_external_Focal.git Focal

./vendor/cm/get-prebuilts
. build/envsetup.sh
breakfast grouper
echo 'PRODUCT_PACKAGES += Focal' >> device/asus/grouper/device.mk
```

if you only want to build Focal, do
```
cd packages/apps/Focal
mmm
```

Otherwise, go to `$ANDROID_HOME` and type `brunch grouper`
