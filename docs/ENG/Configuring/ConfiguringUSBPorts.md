## Configuring USB ports

I've heard that if the Hackintosh is not configured USB ports, it can be different problems, for example, such as

* Lack of recognition of USB3 ports
* Working USB3 ports at a speed USB2 ports
* Slow speed of charging gadgets via USB
* *Perhaps something else is affected by not configured USB ports in the Hackintosh*

---

How to check if you need to configure USB ports? I do not know. I heard that some people tried to charge the iPad from USB ports on the Hackintosh and it charged slowly, and then they somehow set up their Hackintosh and the iPad began to charge faster (perhaps this is not due to this setting of USB ports). You can also try to find out the speed of reading and writing to a USB flash drive that supports USB 3, connecting it to the USB 3 port and tested using the program Blackmagic Disk Speed Test or AJA System Test Lite. My USB 3.0 stick after configuring the USB ports, the program Blackmagic Disk Speed Test showed a read speed of about 12-15 MB/s, and the write speed of 90-100 MB / s.

---

There may be unsupported USB 3 ports on your motherboard, then you need to make sure that macOS supports them before setting them up. This is normally used kekst [XHCI-unsupported](https://github.com/RehabMan/OS-X-USB-Inject-All) (you will need to download the entire repository to get this kekst).

Perhaps on your motherboard a lot of USB ports (like, USB 3 ports OS recognizes not as 1 port, but as a few, but I do not know exactly), but macOS does not recognize more than 15 USB ports (I do not know why). To fix this, you will need to use "patch to limit USB ports to Mojave" [link 1](https://ihackline.com/архив/sg-usb-patcher/) [link 2](https://androidp1.ru/zavod-i-nastrojka-usb-portov-na-hakintosh/).

If you do not have a lot of USB ports and they are all supported in mac OS, for lazy USB port settings simply put kext [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) in folder `/EFI/CLOVER/kexts/Other` and reboot PC.

---

After installing hackintosh and its basic configuration, I did not notice any problems with USB ports, but I did not test them and did not try to find out if there are any problems with them. However, to be sure, I added the kext [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) in `/EFI/CLOVER/kexts/Other`.

My friend advised me to configure the USB ports in the right way by creating a kext USBPorts and several SSDT files. The difference between this method and just using kext [USBInjectAll](https://bitbucket.org/RehabMan/os-x-usb-inject-all/downloads/) I didn't notice (on this system unit).

If you still want to try to configure the USB ports correctly, you can do it manually: https://www.tonymacx86.com/threads/guide-10-11-usb-changes-and-solutions.173616/ whether in semi-automatic mode using the program [Hackintool](https://www.insanelymac.com/forum/topic/335018-hackintool-v280/).

I used the Hacktool program.

First, you need to patch the DSDT file, or enter the appropriate patches in the config, so that the loader itself patches the DSDT during boot.

I acted on [that](https://translate.google.com/translate?js=n&sl=auto&tl=en&u=https://blog.daliansky.net/Intel-FB-Patcher-tutorial-and-insertion-pose.html) instructions from Daliansky. To configure USB, entered in config these patches. I do not know if they are all necessary, but I think they are not mutually exclusive and can be used together.

* XHCI to XHC
* XHCI1 to XHC
* H_EC to EC
* ECDV to EC
* EHC1 to EH01
* EHC2 to EH02
* EC0 to EC

![USB ports DSDT patches Clover Configurator](https://github.com/Drovosek01/hackintosh_DELL_Vostro_3267_i5-6400/blob/master/images/usb_ports/usb_DSDT_patches_CCG.png?raw=true)

![USB ports DSDT patches text config](https://github.com/Drovosek01/hackintosh_DELL_Vostro_3267_i5-6400/blob/master/images/usb_ports/usb_DSDT_patches_text-config.png?raw=true)

Then I rebooted the PC, launched Hackintool and opened the USB tab. It displayed a list/table of USB ports and some of them (half or most) were highlighted in green. Then I took a USB flash drive that supports data transfer standard USB 3.0, took out the keyboard and mouse from the PC and began to insert this stick into all USB ports in turn. Then I took a USB stick that supports only USB 2.0 and also began to insert and remove it in all USB ports.

Then I removed from the list/table all the rows that were not highlighted in green (select the row at the bottom and click on the round button with a minus). Next, all the ports that had the name SS and the transfer rate of 5 Gbps I marked in the table as *USB 3* ports (in the Connector column). The line, which was IOUSBHostDevice (most likely bluetooth) and USB2.0-CRW (most likely it is the Card Reader) I marked it as *Internal* ports. And all the rest I marked as *USB 2* ports (all in the same column Connector).

Then I clicked on the *Export* button (it is there at the bottom), pointed to the folder and the program generated several files:

* USBPorts.kext
* SSDT-EC.aml
* SSDT-UIAC.aml
* SSDT-USBX.aml
* config.plist

In the generated config file.plist were only the patches I mentioned above. If those patches you have applied, then this plist file can simply be removed.

SSDT files need to be moved to a folder `/EFI/CLOVER/ACPI/patched`, and USBPorts.kext needs to be moved to `/EFI/CLOVER/kexts/Other` and then remove kekst USBInjectAll.kext.

After that you need to restart the PC and, theoretically, you will have to work properly USB ports in Hackintosh, but as I said, I have not noticed any changes on this PC.

---

Here is a list of patches for config.plist I used:

```xml
<dict>
	<key>Comment</key>
	<string>Rename XHCI to XHC (USB)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>WEhDSQ==</data>
	<key>Replace</key>
	<data>WEhDXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename XHC1 to XHC (USB)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>WEhDMQ==</data>
	<key>Replace</key>
	<data>WEhDXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename H_EC to EC (USB Power)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>SF9FQw==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>Rename ECDV to EC (USB Power)</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUNEVg==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EHC1 to EH01</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUhDMQ==</data>
	<key>Replace</key>
	<data>RUgwMQ==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EHC2 to EH02</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUhDMg==</data>
	<key>Replace</key>
	<data>RUgwMg==</data>
</dict>
<dict>
	<key>Comment</key>
	<string>change EC0 to EC</string>
	<key>Disabled</key>
	<false/>
	<key>Find</key>
	<data>RUMwXw==</data>
	<key>Replace</key>
	<data>RUNfXw==</data>
</dict>
```

