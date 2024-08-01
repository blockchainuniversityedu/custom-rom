# custom-rom
This repository gives access to the general community development of a custom ROM based on the blockchain. The starting point for its development will be with the Samsung Galaxy J4, as its Exynos 7570 processor does not yet have a testable ROM or kernel despite how long its been around for.

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

To perform a minimal AOSP build, the minimal.xml was created in the same directory as the SSD responsible for the project. File can be found in git repository.

**UPDATE #6**: The lineageOS repository has been installed, and will temporarily replace further developments with AOSP until a testable version of the custom ROM has been built and optimized for the Samsung Galaxy J4 phone architecture. The commands to initiate this process is as follows:

// Create directory for custom-rom //

mkdir /mnt/(drive letter)/(name folder whatever you like)

cd /mnt/(drive letter)/(name folder whatever you like)

// Download and Initialize Repository
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo

chmod a+x ~/bin/repo

repo init -u https://github.com/LineageOS/android.git -b lineage-18.1

// Sync The Repository //

repo sync --no-clone-bundle --no-tags --optimized-fetch --prune -j4

// SSH Key Configurations //
As of August 13th, 2021, GitHub no longer allows password authenticated git clones for repositories from the website. In order to solve this issue, you essentially need to administer your own SSH key via Ubuntu terminal and add it to your GitHub account under the 'SSH and GPG' keys section in 'Settings'. The following are the EXEMPLARY commands to make this happen, as I will obviously not share my private SSH key.

ssh-keygen -t rsa -b 4096 -c "(include your GitHub email here)"

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/id_rsa

cat ~/.ssh/id_rsa.pub --> (From here just copy the SSH key that was generated and link it to your GitHub account)

// Clone the device tree, kernel, and vendor repositories for the Galaxy J4 //
Sidenote: This section initially had repositories provided by lineageOS, but the custom ROM no longer supports this device, hence why you must access its device tree files via the repository offered by TeamWin which you can find a link to below.

git clone git@github.com:TeamWin/android_device_samsung_j4lte.git

// Source The Environment Setup //

source build/envsetup.sh

// Select Target Device //

lunch omni_j4lte-userdebug

// Build ROM, flash it to device once finished //

make -j4 bacon

**UPDATE #7**: Both the AOSP and lineageOS repositories have been installed with minor errors with synchronizing some libraries. The next step is to fix the configuration errors under lineageOS when it comes to setting up a compatible Samsung Galaxy J4 directory under 'devices'. The best way to begin with this approach is to assign the <manufacturer> variable which in this case is 'samsung'

**UPDATE #8**: The code above for the Galaxy J4 configurations have been updated to no longer make use of lineageOS repositories since as of now they don't exist.
