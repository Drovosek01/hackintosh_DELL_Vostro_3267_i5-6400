### Configure Wi-Fi

In this system the unit is installed module `DW1707`. As far as I know, this is a combined Wifi + bluetooth module based on Qualcomm Atheros AR9565. Starting with some version of macOS, Apple refused to support Atheros modules, so in Mojave you need to take the element that is responsible for supporting Atheros modules from the previous version of macOS.

#### How to make it work in macOS AR9565 Mojave 10.14.x

There are several ways to do this, ranging from the most effective (in my opinion) and ending with the most automated. All these methods do about the same thing, only in different ways.

##### Method 1

1. You need to download the patched kext ' AirPortAtheros40.kext ` for AR9565 for macOS Mojave (for example, user **chunnann** posted various versions of this kext here on the forum [insanelymac](https://www.insanelymac.com/forum/topic/312045-atheros-wireless-driver-os-x-101112-for-unsupported-cards/?page=17&tab=comments#comment-2509900))
2. Then you need to put `AirPortAtheros40.kext ` inside another kext in `/System/Library/Extensions/IO80211Family.kext/Contents/PlugIns ` (you will likely need to enable show hidden files and folders, for example, using the program [ShowHiddenFiles](https://gotoes.org/sales/ShowHiddenFilesMacOSX/How_To_Show_Hidden_Files.php))
3. And rebuild the kernel cache/cache kexts (I've done it - just run the program [KextUtility](http://cvad-mac.narod.ru/index/0-4) and wait until the "Quit" button will become active and click on it).

##### Method 2

Use a special script specifically for AR9565 module of the threads on [insanelymac](https://www.insanelymac.com/forum/topic/328426-qualcomm-atheros-ar9565-wireless-for-os-x-108-1014/)

> Open the terminal, copy the code below line by line and press "Enter" after inserting each line
>
> ```bash
> curl -O https://raw.githubusercontent.com/kalifans/Darwin/Driver/ar956x-drv-osx.tar.gz
> tar zxvf ar956x-drv-osx.tar.gz
> cd ar956x-drv-osx
> ./ar956x-inst.sh
> ```

##### Method 3

Use a special program for modules with Atheros forum [insanelymac](https://www.insanelymac.com/forum/files/file/956-atheros-installer-for-macos-mojave-and-catalina/)

##### Method 4

Use a special program from the social network Vkontakte: https://vk.com/wall-135785821_1712

* AR946x: https://yadi.sk/d/SMSlrmt-BF4sMA
* AR9485: https://yadi.sk/d/Fw-v6EKbTUB8jg
* AR9565: https://yadi.sk/d/STSUoAGqi0MHcA

After applying one of these methods, you need to restart your computer.