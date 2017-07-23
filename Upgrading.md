# Upgrading your ASUS P8Z68-V PRO/GEN3 Hackintosh

Upgrading your Hackintosh with an [ASUS P8Z68-V PRO/GEN3](https://www.asus.com/Motherboards/P8Z68V_PROGEN3/) to macOS 10.12 Sierra.

# Preface

This is a guide to updating from an existing El Capitan installation (assuming you've followed the main guide).

If you're just starting off on a new installation, the instructions from the main README should work mostly fine, but I wouldn't recommend using Multibeast to install the proper kexts.

Instead, you should just need AppleIntelE1000e.kext as well as FakeSMC.kext, which should be put in your EFI partition: /EFI/CLOVER/kexts/Other. You can find these online.

## Preparation for the Update

Create a backup of your Hackintosh. I'd suggest using a tool like [Carbon Copy Cloner](https://bombich.com/) to back it up onto an empty drive.

Once it's done backing up, mount your Hackintosh's EFI partition (like discussed in the README) and copy the EFI folder to your desktop.

Unmount your Hackintosh's EFI partition and mount the backup's EFI partition. Copy the EFI folder from your desktop into the EFI partition that you've mounted. You'll probably want to keep the folder on your desktop for now.

This should make your backup a bootable copy of your current Hackintosh. Verify this by attempting to boot from the drive in Clover.

If you ever run into issues on your main drive with the Hackintosh, you will now be able to restore via this drive or instead boot into this drive in order to make the necessary changes to fix your main drive (such as restoring old files).

## Upgrade Clover

Download and install the latest version of [Clover](https://sourceforge.net/projects/cloverefiboot/). You can try installing new drivers, but you may or may not have issues.

Definitely mount your EFI partition and see what's in /EFI/Clover/drivers64UEFI/ both before and after installation, noting any differences as they may cause issues. Even a minor driver update could break things.

If you can't boot after updating, boot up into your backup, mount your Hackintosh's EFI partition, and restore the old drivers based on the EFI folder that's on your desktop and/or on your backup's EFI partition. NOTE: You'll want to boot into your backup's Clover (not your main Hackintosh's Clover) from the BIOS.

## Move your kexts in preparation for the update

Any kexts that you've added should be put into /EFI/CLOVER/kexts/Other, NOT /Library/Extensions. If you've followed the main guide, that means you'll have to move AppleIntelE1000e.kext, FakeSMC.kext, and realtekALC.kext. Copy them into the clover folder and delete them from the old location.

Restart to confirm that the computer is still working as you'd expect. If not, you'll probably want to update and move the kexts back into /Library/Extensions after.

# Updating

See [this guide](https://www.tonymacx86.com/threads/direct-update-to-macos-sierra-using-clover.201465/) for the basics.

1) Download *Install macOS Sierra* from the App Store
2) Choose your Hackintosh's system drive for the install
3) When the Clover boot screen appears, choose the *Boot macOS Install* option. Without caches is not an option for Sierra
4) Wait. When done, your computer should reboot and you should see your normal boot option for your Hackintosh's system drive

Things should be working! If you have issues with your graphics card (video) or audio, see below.

## Fixing Video (no video output with Nvidia cards)

See [this thread](https://www.tonymacx86.com/threads/solving-nvidia-driver-install-loading-problems.161256/) and [this thread](https://www.tonymacx86.com/threads/new-method-for-enabling-nvidia-web-drivers-in-clover.202341/) for more information.

1) Select the "nv_disable=1" option when booting in Clover. This should disable all graphics acceleration, making it run like crap, but you should now have video output so you can fix it.
2) Install [Lilu.kext](https://github.com/vit9696/Lilu) and [NvidiaGraphicsFixup.kext](https://sourceforge.net/projects/nvidiagraphicsfixup/)
3) If using Nvidia web drivers, make sure you're on the latest updated version for Sierra.
4) If using Nvidia web drivers, make sure you follow [this guide](https://www.tonymacx86.com/threads/new-method-for-enabling-nvidia-web-drivers-in-clover.202341/) to use the correct flag for Sierra. This means removing `nvda_drv=1` as a boot arg and adding `NvidiaWeb` in `SystemParameters`.

## Fixing Audio (in/out devices not showing up)

Remove the old realtekALC.kext and install [AppleALC.kext](https://github.com/vit9696/AppleALC/releases). Note if you're using v1.1.0 or higher, you also must install [Lilu.kext](https://github.com/vit9696/Lilu). You may need to add Audio Inject flags into your Clover config. See [this thread](http://www.insanelymac.com/forum/topic/312821-1012-and-alc892/) for more information.

If you were previously using realtekALC.kext, you'll want to delete any dict in your KextsToPatch section in your config.plist that contains "AppleHDA". This will get rid of all of your old realtekALC patches.

For complete details, see [this guide](https://www.reddit.com/r/hackintosh/comments/4sil5p/audio_mechanic_old_patchfix_removal_applealc/).
