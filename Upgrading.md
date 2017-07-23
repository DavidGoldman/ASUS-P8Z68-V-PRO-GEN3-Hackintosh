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

If you can't boot after updating, boot up into your backup, mount your Hackintosh's EFI partition, and restore the old drivers based on the EFI folder that's on your desktop and/or on your backup's EFI partition.

## Move your kexts in preparation for the update

Any kexts that you've added should be put into /EFI/CLOVER/kexts/Other, NOT /Library/Extensions. If you've followed the main guide, that means you'll have to move AppleIntelE1000e.kext, FakeSMC.kext, and realtekALC.kext. Copy them into the clover folder and delete them from the old location.

Restart to confirm that the computer is still working as you'd expect. If not, you'll probably want to update and move the kexts back into /Library/Extensions after.
