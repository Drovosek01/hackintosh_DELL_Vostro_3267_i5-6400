### Setup the config file.plist

Here is described how I have changed net config that goes along with Clover revision 5033.

You can download the disk image with this config from the official page of the loader Clover: https://sourceforge.net/projects/cloverefiboot/files/Bootable_ISO/ For unpacking .lzma and .tar files in Windows use [7zip](https://www.7-zip.org/) or [Bandizip]([https://bandisoft.com/bandizip](https://www.bandisoft.com/bandizip))

Here's what I changed in my [config.plist](/CLOVER/config.plist):

```xml
// TODO
```

This is not a working config.plist. I brought only those items that have been modified compared to the native configuration file of the ISO image Clover revision 5033.

Such SMBIOS will be suitable for loading, but then it is better to generate its points completely, for example with the help of Clover Configurator.

Also, you should read the book "[Clover khaki](https://sourceforge.net/projects/cloverefiboot/files/Documents/)" to understand which items in config.plist responsible for what.