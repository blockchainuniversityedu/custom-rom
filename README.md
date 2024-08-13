# custom-rom
PLEASE READ: This repository gives access to the general community development of a custom ROM based on the blockchain. The starting point for its development will be with the Samsung Galaxy J4, as its Exynos 7570 processor does not yet have a testable ROM or kernel despite how long its been around for. The minimal AOSP build is going to take up an estimate of 130GB in storage, and will take about 30-40 minutes in download time. 

**UPDATE #1**: The repo sync via AOSP installation is at 3% and should be completed later

**UPDATE #2**: According to this article: (https://www.reddit.com/r/AOSP/comments/o5xz8h/warning_as_of_62022_mirroring_aosp_requires_630gb/), the AOSP library can take up to 650GB of storage which can be excessive. It would've been ideal to limit the dependencies to 4 parallel jobs via the 'repo sync -j4' or 'repo sync --depth=1' function but there might be an alternative solution. 

Alternative Solution #1: Use Visual Studio Code, and download Android libraries that could emulate and port over the custom ROM somehow to a rooted cellphone such as the Galaxy J4. More updates soon.

Alternative Solution #2: Using the JBOD method (Just A Bunch of Disks), combine two complementary SSD volumes together to allow for 1 TB of storage access for AOSP library installation, and aim to upgrade current PC storage in the meantime.

**UPDATE #3**: It will be necessary to add a feature where the decentralized ROM will encrypt the root of the phone's device tree with SHA-256 encryption, as using older methods such as MD5 could pose risks with a hash collission. SHA-256 will also allow for quick access to cryptocurrency mining, as well the use of smart contracts if dApps end up getting integrated early on in development.

**UPDATE #4**: Another major feature of the decetralized ROM, will be that its custom-built kernel will be able to make use of the phones device blocks (via built-in hard drive) to mine for ChiaCoin. This could possibly improve or even save energy life for older or recent phones, since hard drive currency mining is now a thing.

**UPDATE #5**: The first alternative solution will be used for likely programming a custom dApp that serves of great purpose to the blockchain ROM. In the meantime, a new name has been given to the custom ROM which will be DiCentra ROM. After doing some troubleshooting, the AOSP installation comes either in 'Full - 12 Parallel Jobs' or 'Minimal - 4 Parallel Jobs', and there are existing Blockchain SDKs, so the best path to take here is to perform a minimal repo sync via command:

// Install Ubuntu command line //

wsl --install

// Install and update packages //

sudo apt-get update

sudo apt-get install openjdk-11-jdk git-core gnupg flex bison gperf \
    build-essential zip curl zlib1g-dev gcc-multilib g++-multilib \
    libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev \
    lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4


// Installing Repo Tool //

mkdir ~/bin

curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

chmod a+x ~/bin/repo

export PATH=~/bin:$PATH

// Adding Repo Tool to your PATH //

echo 'export PATH=~/bin:$PATH' >> ~/.bashrc

source ~/.bashrc

// Initializing and Installing AOSP Repository //

cd /mnt/(drive letter) - For external space

mkdir aosp

cd aosp

repo init -u https://android.googlesource.com/platform/manifest -b master --depth=1

repo sync --no-clone-bundle --no-tags --optimized-fetch --prune -j4

Sidenote: "4 parallel jobs" refers to the number of simultaneous processes or threads used to perform a task

// Downloading the Samsung Galaxy J4 Device Tree - 2 METHODS //

METHOD 1: Via Command Line

// SSH Key Configurations //
As of August 13th, 2021, GitHub no longer allows password authenticated git clones for repositories from the website. In order to solve this issue, you essentially need to administer your own SSH key via Ubuntu terminal and add it to your GitHub account under the 'SSH and GPG' keys section in 'Settings'. The following are the EXEMPLARY commands to make this happen, as I will obviously not share my private SSH key.

ssh-keygen -t rsa -b 4096 -c "(include your GitHub email here)"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub --> (From here just copy the SSH key that was generated and link it to your GitHub account)

// Clone the device tree, kernel, and vendor repositories for the Galaxy J4 //
Sidenote: This section initially had repositories provided by lineageOS, but the custom ROM no longer supports this device, hence why you must access its device tree files via the repository offered by TeamWin which you can find a link to below.

git clone git@github.com:TeamWin/android_device_samsung_j4lte.git

METHOD 2: Download, copy and paste
Visit the HTTPS linke (https://github.com/TeamWin/android_device_samsung_j4lte.git) and download the ZIP file in a matter of seconds. Once finished, enter the aosp folder, go to <device>, create a new folder named 'samsung', and inside of it another folder with the codename of the device tree linked above 'j4lte'. Once finished, extract the contents of the downloaded ZIP folder under the NORMAL extract files option once right clicked, as 7-Zip gives an error when extracting the folder.

// Source The Environment Setup //

source build/envsetup.sh

// Creating the correct lunch combos //
Sidenote: A lunch combo is simply a command written in the 'AndroidProducts.mk' file of the downloaded repository, in order to locate the correct target device build that will be debugged for the custom ROM.

// Select Target Device //
Sidenote: Make sure that 'omni_j4lte-userdebug' and 'omni_j4lte-userdebug' are lunch commands included in files 'AndroidProducts.mk', 'device.mk, and 'vendorsetup.sh' in both the device/samsung/j4lte/ and /build/target/products folder. For the 'envsetup.sh' file, automatically it's given restricted permissions so use commands 'cd <folder directory>/AOSP/build/target/' and 'nano envsetup.sh' to edit files.

lunch aosp_arm64-eng

// Administering Makefiles //
Sienote: Sometimes, Makefiles are not typically installed into the ~/aosp directory, so it would be best to cover some basic commands for this before executing the AOSP build. The commands are as follows (credit: https://askubuntu.com/questions/103348/error-when-installing-makefile-make-no-targets-specified-and-no-makefile):

sudo apt install autoconf automake libpcre3-dev libnl-3-dev libsqlite3-dev libssl-dev ethtool build-essential g++ libnl-genl-3-dev libgcrypt20-dev libtool python3-distutils

sudo apt install -y pkg-config

sudo autoreconf -i

sudo ./configure --with-experimental --with-ext-scripts

sudo make

sudo make install

PLEASE CUT TO **UPDATE #9** in order to make further modifications to Makefiles for a ROM build process without error.

// Downloaded Android 9 AOSP zip folder //

Download Location: https://github.com/phhusson/treble_experimentations/releases/tag/v119

Extract the contents of the ZIP folder, and transfer it over to the root of the AOSP project.

Source Page: https://xdaforums.com/t/aosp-9-0-2019-08-25-phh-treble.3831915/

// Build The ROM, flash over to rooted Android device or use an emulator //

chmod +x build.sh build-rom.sh

./build.sh

./build-rom.sh

All of the system and boot image files should be found in the device directory under /out/

**UPDATE #6**: Both the AOSP and lineageOS repositories have been installed with minor errors with synchronizing some libraries. The next step is to fix the configuration errors under lineageOS when it comes to setting up a compatible Samsung Galaxy J4 directory under 'devices'. The best way to begin with this approach is to assign the <manufacturer> variable which in this case is 'samsung'

**UPDATE #7**: The general guide to running correct commands for installing and building the custom AOSP ROM has been completed for those who want to assist with development, as welcomed. Previously, lineageOS was going to be the early testing stage for dicentra-rom, but unfortunately due to missing repositories on behalf of the organization, the TeamWin repo was helped with building the custom ROM. What's next is to begin the initial blueprints of what's necessary for building dicentra-rom.

**UPDATE #8**: The ROM build command produced the following error via command line which will get fixed:

make[1]: *** No rule to make target 'main.c', needed by 'main.o'.  Stop.

Once finalized, ROM build files will be uploaded to repository for testing.

**UPDATE #9**: The problem with the Makefiles have been solved, and the solution was as follows.

Under the 'Makefile.am' file, add the following pieces of code:

bin_PROGRAMS = main

main_SOURCES = main.c


You should also run these commands to automatically generate any other missing files:

sudo apt-get install automake

automake --add-missing

automake --version

If running make -j$(nproc) gives this error 'make[1]: *** No rule to make target 'main.c', needed by 'main.o'.  Stop.', you simply need to add this code snippet inot the 'Makefile' file.

make.o: main.c

    gcc -c main.c -o main.o

Since automake won't necessarily generate all necessarily missing files, the few to keep to create in the root of the directory are also foo.c, bar.h, and main.c. The code for each file snippet are as follows:

foo.c :

#include <stdio.h>

int main() {

    printf("Hello, World!\n");
    
    return 0;
    
}

bar.h :

#ifndef BAR_H

#define BAR_H


// Add your declarations here

#endif /* BAR_H */

main.c :

#include <stdio.h>

int main() {

    printf("Hello from main!\n");
    
    return 0;
    
}

**UPDATE #10**: Despite all errors being fixed regarding the Makefiles, the lunch and build commands are not seemingly generating the system images that are needed, despite having the device tree files set in place. This might have to do with an error regarding the minimal AOSP build. The 'make -j4' function is seemingly supposed to generate the '/out' folder which includes the system.imng and boot.img files. These do not appear seemingly. Worst case scenario, a full AOSP build might be needed to make everything fully functional, as writing commands such as 'make help' aren't remotely able to showcase what's possible missing from the directory.

**UPDATE #11**: According to a Telegram group chat responsible for jailbreaking the Samsung Galaxy J4, GSI (Generic System Image) ROMs are available to be compatible with the smartphone device. If the minimal AOSP build is not capable of building the ROM, I will most likely have to download a GSI ROM for a separate set of devices available via (https://developer.android.com/topic/generic-system-image), and simply edit its contents to eventually develop features that will make up of DiCentra ROM. Once the features are solidified, I will simply have to edit the contents of the 'device/j4lte' folder in order to make sure that the modified GSI ROM will be able to flash over to the emulator for further testing within LOWER END devices. Part of the challenge will be to make sure that the modified GSI ROM will be compliant with the settings of the Samsung Galaxy J4.

**UPDATE #12**: After doing some searching, I discovered a forum that supported AOSP with GSI ROMs functioning under Android 9 Pie, which is the popular version of OS found in most Galaxy J4 devices if not for the earliest version, Android 8 Oreo. This XDA forum provides the ZIP file for the needed build files that would've inititally been replaced by the 'make -j$(nproc)' command. I have updated the steps above in order to correct build a compatible ROM under the TWRP-GalaxyJ4 kernel provided.

**UPDATE #13**: After repeating the 'lunch omni_j4lte-userdebug' command a multitude of times, it became evident that the command itself more or less refers to an outdated, open source project called OmniROM which might not necessarily speak for phones past the Galaxy J4. As the processor for the J4, the Exynos 7570, works with ARM64, I've changed the lunch combo to more recognizable with that command in order to avoid further errors. 
