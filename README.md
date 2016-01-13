# ASUS P8Z68-V PRO/GEN3 Hackintosh

Hackintosh for [ASUS P8Z68-V PRO/GEN3](https://www.asus.com/Motherboards/P8Z68V_PROGEN3/) using OS X 10.11 El Capitan.

Intel Z68 chipset, LGA1155 socket.  
Supports 2nd gen. ([32 nm - Sandy Bridge](http://en.wikipedia.org/wiki/Sandy_Bridge)) Intel Core CPUs.

Onboard devices:
- Intel 82579
- Realtek ALC892
- ASMedia USB 3.0 controller

## BIOS Settings

I used the latest stable BIOS (3802 at this time) from [ASUS](https://www.asus.com/Motherboards/P8Z68V_PROGEN3/HelpDesk_Download/).

Make sure you're using the default settings (F5: Optimized Defaults). Also make sure that "Normal Mode" is selected on the EZ Mode. Overclocking can mess with the power management later on and led my hackintosh to crashes on boot (P-state Stepper Error 18 at Step 2 in context 2 on CPU).

## Patched BIOS to avoid Kernel Panic

For proper powermanagement, I [patched](http://www.insanelymac.com/forum/topic/285444-uefipatch-uefi-patching-utility/) the BIOS using [UEFIPatch](https://github.com/LongSoft/UEFITool/releases/tag/0.21.5). You might be able to avoid this, but I'm not sure. Clover does come with an "AsusAICPUPM" setting which should avoid the crash but I'm unusure if it will work with sleep/wake.

You can also use NullCPUPowerManagement.kext but you won't have Intel SpeedStep and the computer won't be able to sleep.

You can flash your BIOS using the "ASUS EZ Flash 2 Utility" found in your BIOS.

## Unibeast

I followed the [tonymacx86 guide](http://www.tonymacx86.com/el-capitan-desktop-guides/172672-unibeast-install-os-x-el-capitan-any-supported-intel-based-pc.html).
- Bootloader Options -> UEFI Boot Mode

You may need to add "cpus=1" to the CLOVER config to be able to complete boot on the OS X install. I used EFI Mounter v3 to [mount the EFI partition](http://www.tonymacx86.com/basics/174321-how-mount-efi-partition.html) and then edited EFI/CLOVER/config.plist and added cpus=1 to the boot arguments. You can use the [Clover configurator](http://mackie100projects.altervista.org/clover-configurator/) to edit the config file or edit it [manually](http://clover-wiki.zetam.org/Configuration) in a text editor.

Complete the install (follow the tonymacx86 guide above).

## Prohibited Sign on Boot

Boot in verbose mode (-v as boot arg in the config.plist or add in the Clover options menu or hit spacebar while on the USB/drive and select verbose mode). If you see "Error allocating pages..." like I did, you should try replacing EFI/CLOVER/drivers64UEFI/OsxAptioFix2Drv-64.efi with OsxAptioFixDrv-64.efi (or vice versa). You can download this somewhere on the internet or install Clover to a spare USB (might need to customize your install the get the file) and then copy it over or just reinstall Clover on your USB/drive manually (not using Unibeast), being sure to customize the install to get the file.

## Multibeast 8.x

- Quick Start -> UEFI Boot Mode
- Drivers -> Realtek ALCxxx -> ALC892
- Drivers -> Network -> AppleIntelE1000e
- System Definitions -> iMac -> iMac12,2

This will finish and you:
- can also install the NVIDIA web drivers before restarting. It's working fine for me, but milage may vary based on your graphics card
- should replace EFI/CLOVER/drivers64UEFI/OsxAptioFix2Drv-64.efi with OsxAptioFixDrv-64.efi on the drive if you needed to do so for your USB
- should restart your computer

## Booting from Mac OS X SSD/HDD

For me, the drive where I installed OS X was not bootable (no UEFI option in BIOS). Thus, even after installing Multibeast, I still couldn't boot into Clover without using the USB.

Here's how I fixed it:
- Plug in the USB that you used above
- Boot into the Clover menu with the USB. You should see "Clover Boot Options" twice. One of these is for your USB (should contain \\USB if you view the Boot Options) and then the other is for the drive with OS X
- Select the "Clover Boot Options" corresponding to your drive. Select "Add Clover boot options for all entries"
- Shutdown and unplug USB
- Boot and select the drive in BIOS, booting into Clover. For me, it was named weird (like "UEFI: Clover start boot.efi at *USB*...")
- Clover Boot Options -> Remove all Clover boot options
- Clover Boot Options -> Add Clover boot options for all entries
- Restart. Now I was able to boot Clover properly from my drive (option named "UEFI: Clover start boot.efi at *El Capitan*...")

## USB 3.0

I used RehabMan's [GenericUSBXHCI](https://github.com/RehabMan/OS-X-Generic-USB3) USB 3.0 driver. Install this using [kextbeast](http://tonymacx86.blogspot.com/2010/08/kextbeast-simple-kext-installer.html).

Note that later on, this driver prevented my computer from sleeping - it would immediately wake up aftering going to sleep.

In order to fix this:
- Use the "-gux_nosleep" boot argument. You can manually edit your config.plist or use the Clover Configurator. 

## Power Management

Follow the general guide from [here](http://www.tonymacx86.com/ssdt/177456-quick-guide-generate-ssdt-cpu-power-management.html) (or [here](http://www.tonymacx86.com/mavericks-desktop-support/128926-mavericks-native-cpu-igpu-power-management.html)).
- Deselect any power management selections in Clover Configurator
- Check SSDT/Drop OEM (or try the other option mentioned in the second guide)

Note that the ssdtPRGen requires a working internet connection (or for you to [predownload](https://github.com/Piker-Alpha/ssdtPRGen.sh) the required files).

## Using iMessage

Following the instructions given [here](http://www.tonymacx86.com/general-help/110471-how-fix-imessage.html).

Be sure to do the following:
- Use Clover Configurator's Magic Wand (with iMac 12,2)
  * Be sure to use the **two shake buttons** provided when generating the SMBIOS
  * Use uuidgen to fill in a value for the 'SmUUID'
  * Follow the instructions on the guide for manual MLB & ROM values
- Clear the cache according to the guide

## Other Stuff

- Windows displaying the wrong time after booting into OS X? Follow [this guide](http://lifehacker.com/5742148/fix-windows-clock-issues-when-dual-booting-with-os-x)
