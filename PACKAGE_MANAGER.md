# Linux distributions
- Debian
  - Ubuntu
- Red Hat
  - Red Hat Enterprise
  - Fedora
  - CentOS
- Arch Linux
- Slackware
  - OpenSUSE
- Different kind of beast: FreeBSD
- Comprehensive list
  - table: https://en.wikipedia.org/wiki/Comparison_of_Linux_distributions#General
  - image: https://en.wikipedia.org/wiki/List_of_Linux_distributions

# Package managers
  - Comprehensive list of PMs based on distros: https://en.wikipedia.org/wiki/Comparison_of_Linux_distributions#Package_management_and_installation

# apt
- Repository
  - sources.list
    - Overview: https://wiki.debian.org/SourcesList
    - Where it is
      - /etc/apt/sources.list
      - /etc/apt/sources.list.d/*
    - How it looks
      ```
      deb http://site.example.com/debian distribution component1 component2 component3
      deb-src http://site.example.com/debian distribution component1 component2 component3
      ```
  - main, restricted, universe, multiverse: https://askubuntu.com/questions/401941/what-is-the-difference-between-security-updates-proposed-and-backports-in-etc
    - rpi has different names for this, maybe obviously? something like:  
      ```
      deb http://raspbian.raspberrypi.org/raspbian/ stretch main contrib non-free rpi
      ```
  - mirror
    - Now it's way easier to configure fastest mirror
      ```shell
      $ \
      (
      CODE_NAME=$(lsb_release -cs); echo "deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME} main restricted universe multiverse
      deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-updates main restricted universe multiverse
      deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-backports main restricted universe multiverse
      deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-security main restricted universe multiverse" > /etc/apt/sources.list
      )
      ```
      - Output(without mirror):
        ```shell
        root@8adc8aee8e7c:/# \
        > (
        > CODE_NAME=$(lsb_release -cs); echo "deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME} main restricted universe multiverse
        > deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-updates main restricted universe multiverse
        > deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-backports main restricted universe multiverse
        > deb mirror://mirrors.ubuntu.com/mirrors.txt ${CODE_NAME}-security main restricted universe multiverse" > /etc/apt/sources.list
        > )

        root@8adc8aee8e7c:/# time apt-get update
        Ign http://mirror.kakao.com/ubuntu/ trusty InRelease
        Get:1 http://mirror.kakao.com/ubuntu/ trusty-updates InRelease [65.9 kB]
        Get:2 http://mirror.kakao.com/ubuntu/ trusty-backports InRelease [65.9 kB]
        ...
        Get:24 https://esm.ubuntu.com trusty-infra-security/main amd64 Packages
        Get:25 https://esm.ubuntu.com trusty-infra-updates/main amd64 Packages
        Fetched 13.9 MB in 3s (3698 kB/s)
        Reading package lists... Done

        real  0m4.761s
        user  0m2.121s
        sys 0m0.297s
        ```
      - Output(with mirror):
        ```shell
        root@3610d47776a7:/# time apt-get update
        Get:1 http://security.ubuntu.com trusty-security InRelease [65.9 kB]
        Ign http://archive.ubuntu.com trusty InRelease
        Get:2 http://archive.ubuntu.com trusty-updates InRelease [65.9 kB]
        ...
        Get:22 http://archive.ubuntu.com trusty/universe amd64 Packages [7589 kB]
        Get:23 http://archive.ubuntu.com trusty/multiverse amd64 Packages [169 kB]
        Fetched 13.8 MB in 9s (1500 kB/s)
        Reading package lists... Done

        real  0m10.264s
        user  0m2.308s
        sys 0m0.363s
        root@3610d47776a7:/#
        ```
  - PPA(Personal Package Archive)
    - It allows developers to deliver/update their personal apps faster
    - Also used as a nightly delivery channel
    - How it's done:
      - Add public key to local apt key file
        ```shell
        # curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
        ```
      - Check if it's added
        ```shell
        # apt-key fingerprint
        ```
        output: 
        ```
        /etc/apt/trusted.gpg.d/ubuntu-keyring-2012-archive.gpg
        ------------------------------------------------------
        pub   rsa4096 2012-05-11 [SC]
              790B C727 7767 219C 42C8  6F93 3B4F E6AC C0B2 1F32
        uid           [ unknown] Ubuntu Archive Automatic Signing Key (2012) <ftpmaster@ubuntu.com>

        /etc/apt/trusted.gpg.d/ubuntu-keyring-2012-cdimage.gpg
        ------------------------------------------------------
        pub   rsa4096 2012-05-11 [SC]
              8439 38DF 228D 22F7 B374  2BC0 D94A A3F0 EFE2 1092
        uid           [ unknown] Ubuntu CD Image Automatic Signing Key (2012) <cdimage@ubuntu.com>

        /etc/apt/trusted.gpg.d/ubuntu-keyring-2018-archive.gpg
        ------------------------------------------------------
        pub   rsa4096 2018-09-17 [SC]
              F6EC B376 2474 EDA9 D21B  7022 8719 20D1 991B C93C
        uid           [ unknown] Ubuntu Archive Automatic Signing Key (2018) <ftpmaster@ubuntu.com>
        ```
      - Add repository
        - You can either add it via tool:
        ```shell
        # apt-get install software-properties-common
        # add-apt-repository \
        "deb [arch=amd64] https://download.docker.com/linux/debian \
        $(lsb_release -cs) \
        stable"
        ```
        - or you can also just edit /etc/apt/sources.list(.d/*)
- Various binaries
  - apt, apt-get, aptitude
    - apt-get/apt-cache kind of stuff came earlier
    - It was kinda messy since there were so many different binaries, like apt-get, apt-cache, apt-mark.
    - So, apt/aptitude was made to provide a familiar interface to users.
    - Most of the time, we can use apt/aptitude over apt-get, but these binaries will manage packages in a slightly different ways, like managing dependencies or what not.
    - Keep in mind of these facts and choose wisely. Try apt/aptitude if you're not familiar with them.
    - The most significant differences between these are:  
      ![Moo...](/images/package%20manager/mooing%20in%20apt-get.png "Mooget...")
      ![Moo...](/images/package%20manager/mooing%20in%20aptitude.png "Mootitude...")
  - apt-mark: Used to mark/unmark whether some package is installed automatically or manually. PM will take this into consideration to decide whether it should automatically uninstall some package or not.
  - apt-cache
    > apt-cache performs a variety of operations on APT's package cache.  apt-cache does not
    manipulate the state of the system but does provide operations to search and generate
    interesting output from the package metadata. The metadata is acquired and updated via the
    'update' command of e.g.  apt-get, so that it can be outdated if the last update is too
    long ago, but in exchange apt-cache works independently of the availability of the
    configured sources (e.g. offline).
- Under the hood of ```apt-get install```
  - // TODO: THIS SECTION IS UNFINISHED...
  - dpkg -L vim-tiny

# References
- https://itsfoss.com/ppa-guide/
- https://docs.docker.com/engine/install/debian/#set-up-the-repository
- Fast mirror: https://askubuntu.com/a/9035
- apt vs apt-get(1): https://linuxhint.com/diff_apt_vs_aptget/
- apt vs apt-get(2): https://phoenixnap.com/kb/apt-vs-apt-get
- apt vs aptitude: https://linuxconfig.org/comparison-of-major-linux-package-management-systems
- apt-mark: http://manpages.ubuntu.com/manpages/bionic/man8/apt-mark.8.html
- apt-cache: http://manpages.ubuntu.com/manpages/xenial/man8/apt-cache.8.html
- what happens under ```apt-get install```: https://askubuntu.com/a/540943
