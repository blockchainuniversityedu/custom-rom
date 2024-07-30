# custom-rom
This repository gives access to the general community development of a custom ROM based on the blockchain. The starting point for its development will be with the Samsung Galaxy J4, as its Exynos processor does not yet have a testable ROM or kernel despite how long its been around for.

Update #1: The repo sync via AOSP installation is at 3% and should be completed later

Update #2: According to this article: (https://www.reddit.com/r/AOSP/comments/o5xz8h/warning_as_of_62022_mirroring_aosp_requires_630gb/), the AOSP library can take up to 650GB of storage which can be excessive. It would've been ideal to limit the dependencies to 4 parallel jobs via the 'repo sync -j4' or 'repo sync --depth=1' function but there might be an alternative solution. 

Alternative Solution #1: Use Visual Studio Code, and download Android libraries that could emulate and port over the custom ROM somehow to a rooted cellphone such as the Galaxy J4. More updates soon.
Alternative Solution #2: Using the JBOD method (Just A Bunch of Disks), combine two SSD volumes together to allow for 1 TB of storage access for AOSP library installation, and aim to upgrade current PC storage in the meantime.
