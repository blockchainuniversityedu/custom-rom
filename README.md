# custom-rom
This repository gives access to the general community development of a custom ROM based on the blockchain. The starting point for its development will be with the Samsung Galaxy J4, as its Exynos 7570 processor does not yet have a testable ROM or kernel despite how long its been around for.

Update #1: The repo sync via AOSP installation is at 3% and should be completed later

Update #2: According to this article: (https://www.reddit.com/r/AOSP/comments/o5xz8h/warning_as_of_62022_mirroring_aosp_requires_630gb/), the AOSP library can take up to 650GB of storage which can be excessive. It would've been ideal to limit the dependencies to 4 parallel jobs via the 'repo sync -j4' or 'repo sync --depth=1' function but there might be an alternative solution. 

Alternative Solution #1: Use Visual Studio Code, and download Android libraries that could emulate and port over the custom ROM somehow to a rooted cellphone such as the Galaxy J4. More updates soon.

Alternative Solution #2: Using the JBOD method (Just A Bunch of Disks), combine two complementary SSD volumes together to allow for 1 TB of storage access for AOSP library installation, and aim to upgrade current PC storage in the meantime.

Update #3: It will be necessary to add a feature where the decentralized ROM will encrypt the root of the phone's device tree with SHA-256 encryption, as using older methods such as MD5 could pose risks with a hash collission. SHA-256 will also allow for quick access to cryptocurrency mining, as well the use of smart contracts if dApps end up getting integrated early on in development.

Update #4: Another major feature of the decetralized ROM, will be that its custom-built kernel will be able to make use of the phones device blocks (via built-in hard drive) to mine for ChiaCoin. This could possibly improve or even save energy life for older or recent phones, since hard drive currency mining is now a thing.

Update #5: The first alternative solution will be used for likely programming a custom dApp that serves of great purpose to the blockchain ROM. In the meantime, a new name has been given to the custom ROM which will be DiCentra ROM. After doing some troubleshooting, the AOSP installation comes either in 'Full - 12 Parallel Jobs' or 'Minimal - 4 Parallel Jobs', and there are existing Blockchain SDKs, so the best path to take here is to perform a minimal repo sync via command:

wsl --install

cd /mnt/(drive letter) - For external space

mkdir aosp

cd aosp

repo init -u https://android.googlesource.com/platform/manifest -b master --depth=1

repo sync --no-clone-bundle --no-tags --optimized-fetch --prune -j4

Sidenote: "4 parallel jobs" refers to the number of simultaneous processes or threads used to perform a task

To perform a minimal AOSP build, the minimal.xml was created in the same directory as the SSD responsible for the project. File can be found in git repository.
