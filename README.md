
# How to build custom OpenWrt packages:
#      UrBackup Server
#      UrBackup Client
#      Samba 3.6
#      miniDLNA (with thumbnails creation)
# for WD My Book Live NAS (with PowerPC cpu)

Package UrBackup server was created by me.
Package UrBackup client was created by me.
Package Samba36 was copied from previos versions of OpenWRT (19...).
Package miniDLNA was copied from Entware repository and modified (https://github.com/Entware/entware-packages).

miniDLNA from Entware is patched for thumbnails creation support.


1. Download Openwrt SDK
[https://downloads.openwrt.org/releases/22.03.2/targets/apm821xx/sata/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz](https://downloads.openwrt.org/releases/22.03.2/targets/apm821xx/sata/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz "https://downloads.openwrt.org/releases/22.03.2/targets/apm821xx/sata/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz")


2. Extract downloaded archive:
```shell
tar xf openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64.tar.xz
```

3. Go to sdk directory:
```shell
cd openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64
```

4. Copy urbackup-server-package directory from this repository to SDK directory

5. Append a line with urbackup-server feed to feeds.conf.default (replace PATH_2_SDK by current path to SDK):
```shell
src-link custompackages /PATH_2_SDK/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64/custompackages
```
for example:

```shell
src-link custompackages /home/username/openwrt-sdk-22.03.2-apm821xx-sata_gcc-11.2.0_musl.Linux-x86_64/custompackages
```


6. Inside SDK directory run:
```shell
./scripts/feeds update -a
./scripts/feeds uninstall -a                        (optional for minidlna (to override existing openwrt package))
./scripts/feeds install -f -p custompackages -a     (optional for minidlna (to override existing openwrt package))
./scripts/feeds install -a
make menuconfig
```

7. In menuconfig GUI:

>     Enter Global Build Settings and in the submenu, deselect/exclude the following options:
>             Select all target specific packages by default
>             Select all kernel module packages by default
>             Select all userspace packages by default
>     Enter Network
>             Select urbackup-server      (for UrBackup Server only)
>             Select urbackup-client      (for UrBackup Client only)
>             Select samba36-client       (for Samba 3.6 only)
>             Select samba36-hotplug      (for Samba 3.6 only)
>             Select samba36-net          (for Samba 3.6 only)
>             Select samba36-server       (for Samba 3.6 only)
>     Enter Multimedia
>             Select minidlna             (for miniDLNA only )
>     Enter LuCI->Applications
>             Select luci-app-samba       (for Samba 3.6 only)
>     Save
>     Exit

10. Build packages:
```shell
make package/urbackup-server/compile
make package/urbackup-client/compile
make package/samba36/compile
make package/luci-app-samba/compile
make package/minidlna/compile
```
if you have errors you can try compile with flag V=s to see what happens. For example:
```shell
make package/urbackup-client/compile V=s
```


11. You can find results in PATH_2_SDK/bin/package/powerpc_464fp/custompackages/



If you want to see verbose log when package is configured:
```shell
make package/urbackup-server/configure V=s
```



