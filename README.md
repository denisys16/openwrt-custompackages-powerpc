How to build package

1. Download Openwrt SDK https://downloads.openwrt.org/releases/22.03.2/targets/apm821xx/sata/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz

2. Extract downloaded archive:
        tar xf openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz
3. Go to sdk directory:
        cd openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64
4. Copy urbackup-server-package directory from this repository to SDK directory

5. Append a line with urbackup-server feed to feeds.conf.default (replace PATH_2_SDK by current path to SDK):
        src-link custompackages /PATH_2_SDK/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64/custompackages
    for example:
        src-link custompackages /home/username/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64/custompackages


6. Inside SDK directory run:
        ./scripts/feeds update -a
        ./scripts/feeds install -a
        make menuconfig

7. In menuconfig GUI:
    Enter Global Build Settings and in the submenu, deselect/exclude the following options:
        Select all target specific packages by default
        Select all kernel module packages by default
        Select all userspace packages by default
    Enter Network
        Select urbackup-server
    Save
    Exit

10. Build package:
        make package/urbackup-server/compile
    if you have errors you can try compile with flag V=s
        make package/urbackup-server/compile V=s


11. You can find rusult in PATH_2_SDK/bin/package/__cpu___/custompackages/



=======================================
If you want to see verbose log when package is configured:
        make package/urbackup-server/configure V=s

To build dependencies:
        make package/zlib/compile
        make package/zstd/compile
        make package/curl/compile
