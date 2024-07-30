# custom-rom
This repository gives access to the general community development of a custom ROM based on the blockchain. The starting point for its development will be with the Samsung Galaxy J4, as its Exynos processor does not yet have a testable ROM or kernel despite how long its been around for.

Update: Here are the following commands in order to enter the process of building the custom ROM (with Mac)

1. Set Up Your Development Environment
Install Android Studio:
Download and install Android Studio from the Android Developer site.
During installation, make sure to install the Android SDK, Android SDK Platform-Tools, and Android Emulator.
Install Git: Git is typically pre-installed on macOS, but you can update or install it via Homebrew

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install git

Install necessary tools using Homebrew:

brew install gnupg flex bison gperf \
  build-essential zip curl zlib xz

Set Up repo Tool: Install the repo tool for managing the Android source code

mkdir ~/bin
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
chmod a+x ~/bin/repo
echo 'export PATH=~/bin:$PATH' >> ~/.zshrc
source ~/.zshrc

2. Download the Android Source Code
Initialize Repo:
Create a directory for the source code and initialize the repo:
mkdir ~/android
cd ~/android
repo init -u https://android.googlesource.com/platform/manifest
repo sync

3. Modify the Source Code
Open the Source Code: Use a code editor like Visual Studio Code to open and modify the source code.

Install Visual Studio Code from here and open the source code directory:
code ~/android
Make Changes: Navigate to the directories you want to modify and make your changes (e.g., UI changes, adding new features).

5. Build the Custom ROM
Set Up the Build Environment: In the terminal, set up the build environment

cd ~/android
source build/envsetup.sh
lunch aosp_arm-eng
Compile the ROM:

Start the build process:
make -j$(sysctl -n hw.ncpu)

5. Test on Emulator
Start Emulator: Use the AVD (Android Virtual Device) Manager in Android Studio to create and start an emulator.
Flash the ROM to Emulator. In your terminal, use adb to push and flash the built images to the emulator:

adb root
adb remount
adb push out/target/product/generic/system.img /system
adb reboot

6. Test on Physical Device
Unlock Bootloader: Ensure your device’s bootloader is unlocked. Follow the specific instructions for your device.
Enter Fastboot Mode: Use adb to reboot into fastboot mode.

adb reboot bootloader

Flash the ROM: Use fastboot to flash the ROM onto the device.

fastboot flash system out/target/product/<device_name>/system.img
fastboot flash boot out/target/product/<device_name>/boot.img
fastboot flash recovery out/target/product/<device_name>/recovery.img
fastboot reboot

7. Iterate
Make Changes: Based on your testing results, make necessary changes to the source code.
Rebuild and Flash:

Repeat the build and flash process to apply your changes.
