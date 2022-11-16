# macOS External Display Fix

A fix for blurry text on external displays in macOS & OS X.

This is based on my [original gist](https://gist.github.com/MethodGrab/ef8d254f469bc85b4d3e5fad3d37b74e).


## Notes

- Entering the recovery mode keyboard combination (<kbd>CMD-R</kbd>) must be done using the laptop keyboard. Using an external keyboard (e.g. Apple Magic keyboard) will **not** work.


## Instructions


### 11 macOS Big Sur / 12 macOS Monterey

1. Download and run the [patch](./patch-edid.rb) ([src][patch-edid-src]) which will create a new directory in the CWD called `DisplayVendorID-XXXX`:

	```shell
	ruby ./patch-edid.rb
	```

1. Copy the patch dir to `/Library/Displays/Contents/Resources/Overrides`:  
	`XXXX` is the suffix of the folder the patch script created.  
	`~/Downloads` is the path `patch-edid.rb` was run in.  
	Note: the path is `/Library/...`, **not** `/System/Library/...`.  

	```shell
	sudo mkdir -p /Library/Displays/Contents/Resources/Overrides
	sudo cp -r ~/Downloads/DisplayVendorID-XXXX /Library/Displays/Contents/Resources/Overrides
	```

1. Reboot.
1. That's it!


#### Notes

- Tested in Big Sur & Monterey.
- Rebooting into safe mode is no longer required!  
  It's also no longer possible because as of Big Sur the file system is mounted read-only in recovery mode which makes editing `/System/Library/Displays/Contents/Resources/Overrides` impossible.


---


### 10.15 macOS Catalina

1. Download and run the [patch](./patch-edid.rb) ([src][patch-edid-src]) which will create a new directory in the CWD called `DisplayVendorID-XXXX`:

	```shell
	ruby ./patch-edid.rb
	```

1. Reboot into recovery mode (hold down <kbd>CMD-R</kbd> while rebooting)
1. Open `Terminal` from the `Utilities` menu  
	Note: If you use FileVault, you will need to [unlock & mount](https://derflounder.wordpress.com/2017/11/04/unlock-or-decrypt-an-encrypted-apfs-boot-drive-from-the-command-line) `Macintosh HD` before continuing:
	1. `diskutil apfs list`
	1. Find the volume identifier for `Macintosh HD`:

		```shell
		APFS Volume Disk (Role):  disk1s1 (No specific role) // `disk1s1` is the volume identifier
		Name:                     Macintosh HD (Case-insensitive)
		Mount Point:              /
		Capacity Consumed:        250GB
		Encrypted:                Yes
		```

	1. `diskutil apfs unlockVolume /dev/volume_identifier` where `volume_identifier` is the one shown in the diskutil listing
	1. After unlocking the disk with an approved FileVault user, `Macintosh HD` will now be mounted at `/Volumes/Macintosh HD`
1. Copy the patch dir to `/System/Library/Displays/Contents/Resources/Overrides/`, I recommend backing up any existing directory if it exists for your display:  
	`XXXX` is the suffix of the folder the patch script created.  
	`YYYY` is the suffix of the file it created.  
	`/Volumes/Macintosh\ HD/Users/me/Downloads` is the path `patch-edid.rb` was run in.  

	```shell
	cd /Volumes/Macintosh\ HD/System/Library/Displays/Contents/Resources/Overrides/
	mv DisplayVendorID-XXXX DisplayVendorID-XXXX-BACKUP
	mkdir DisplayVendorID-XXXX
	cp /Volumes/Macintosh\ HD/Users/me/Downloads/DisplayVendorID-XXXX/DisplayProductID-YYYY ./DisplayVendorID-XXXX
	```

1. Reboot back into normal mode.
1. That's it!


#### Notes

- Disabling System Integrity Protection (SIP) is no longer required in macOS Catalina.


---


### 10.12 macOS Sierra

1. Download and run the [patch](./patch-edid.rb) ([src][patch-edid-src]) which will create a new directory in the CWD called `DisplayVendorID-XXXX`:

	```shell
	ruby ./patch-edid.rb
	```

1. Reboot into recovery mode (hold down <kbd>CMD-R</kbd> while rebooting)
1. [Disable SIP](https://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html):

	```shell
	csrutil disable
	```

1. Reboot into recovery mode (hold down `CMD-R` while rebooting) _again_ so the SIP change takes effect
1. Open `Terminal` from the `Utilities` menu  
	Note: If you use FileVault, you will need to [unlock & mount](https://derflounder.wordpress.com/2017/11/04/unlock-or-decrypt-an-encrypted-apfs-boot-drive-from-the-command-line) `Macintosh HD` before continuing:
	1. `diskutil apfs list`
	1. Find the volume identifier for `Macintosh HD`:

		```shell
		APFS Volume Disk (Role):  disk1s1 (No specific role) // `disk1s1` is the volume identifier
		Name:                     Macintosh HD (Case-insensitive)
		Mount Point:              /
		Capacity Consumed:        250GB
		Encrypted:                Yes
		```

	1. `diskutil apfs unlockVolume /dev/volume_identifier` where `volume_identifier` is the one shown in the diskutil listing
	1. After unlocking the disk with an approved FileVault user, `Macintosh HD` will now be mounted at `/Volumes/Macintosh HD`
1. Copy the patch dir to `/System/Library/Displays/Contents/Resources/Overrides/`, I recommend backing up any existing directory if it exists for your display:  
	`XXXX` is the suffix of the folder the patch script created.  
	`YYYY` is the suffix of the file it created.  
	`/Volumes/Macintosh\ HD/Users/me/Downloads` is the path `patch-edid.rb` was run in.  

	```shell
	cd /Volumes/Macintosh\ HD/System/Library/Displays/Contents/Resources/Overrides/
	mv DisplayVendorID-XXXX DisplayVendorID-XXXX-BACKUP
	mkdir DisplayVendorID-XXXX
	cp /Volumes/Macintosh\ HD/Users/me/Downloads/DisplayVendorID-XXXX/DisplayProductID-YYYY ./DisplayVendorID-XXXX
	```

1. [Re-enable SIP](https://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html):

	```shell
	csrutil enable
	```

1. Reboot back into normal mode.
1. That's it!
1. You can verify that SIP has been re-enabled with:

	```shell
	csrutil status
	```


---


### 10.11 OS X El Capitan

1. Download and run the [patch](./patch-edid.rb) ([src][patch-edid-src]) which will create a new directory in the CWD called `DisplayVendorID-XXXX`:

	```shell
	ruby ./patch-edid.rb
	```

1. Reboot into recovery mode (hold down <kbd>CMD-R</kbd> while rebooting)
1. [Disable SIP](https://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html):

	```shell
	csrutil disable
	```

1. Reboot into recovery mode (hold down `CMD-R` while rebooting) _again_ so the SIP change takes effect
1. Open `Terminal` from the `Utilities` menu  
	Note: If you use FileVault, you will need to [unlock & mount](http://apple.stackexchange.com/a/236341) `Macintosh HD` before continuing:
	1. `diskutil list`
	1. Find the lvUUID:

		```shell
		Logical Volume on disk0s2
		4B2EFAAE-C871-4E6D-AB15-2DDE604B97CE // this is lvUUID
		Locked Encrypted
		```

	1. `diskutil cs unlockVolume lvUUID` where `lvUUID` is the one shown in the diskutil listing
	1. After unlocking the disk with an approved FileVault user, `Macintosh HD` will now be mounted at `/Volumes/Macintosh HD`
1. Copy the patch dir to `/System/Library/Displays/Contents/Resources/Overrides/`, I recommend backing up any existing directory if it exists for your display:  
	`XXXX` is the suffix of the folder the patch script created.  
	`YYYY` is the suffix of the file it created.  
	`/Volumes/Macintosh\ HD/Users/me/Downloads` is the path `patch-edid.rb` was run in.  

	```shell
	cd /Volumes/Macintosh\ HD/System/Library/Displays/Contents/Resources/Overrides/
	mv DisplayVendorID-XXXX DisplayVendorID-XXXX-BACKUP
	mkdir DisplayVendorID-XXXX
	cp /Volumes/Macintosh\ HD/Users/me/Downloads/DisplayVendorID-XXXX/DisplayProductID-YYYY ./DisplayVendorID-XXXX
	```

1. [Re-enable SIP](https://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html):

	```shell
	csrutil enable
	```

1. Reboot back into normal mode.
1. That's it!
1. You can verify that SIP has been re-enabled with:

	```shell
	csrutil status
	```


---


## Sources

- [Unlock or decrypt an encrypted APFS boot drive from the command line](https://derflounder.wordpress.com/2017/11/04/unlock-or-decrypt-an-encrypted-apfs-boot-drive-from-the-command-line)
- [How to modify System Integrity Protection in El Capitan](https://www.macworld.com/article/2986118/security/how-to-modify-system-integrity-protection-in-el-capitan.html)
- [ejdyksen/patch-edid.rb](https://gist.github.com/ejdyksen/8302862)
- [Improved `patch-edid.rb`](https://gist.github.com/adaugherity/7435890)
- [Force RGB mode in Mac OS X to fix the picture quality of an external monitor](http://www.ireckon.net/2013/03/force-rgb-mode-in-mac-os-x-to-fix-the-picture-quality-of-an-external-monitor)
	- [Without disabling SIP](http://www.ireckon.net/2013/03/force-rgb-mode-in-mac-os-x-to-fix-the-picture-quality-of-an-external-monitor/comment-page-11#comment-14866)
- [Instructions for Forcing RGB mode in Catalina](https://www.reddit.com/r/MacOS/comments/dkowz1/instructions_for_forcing_rgb_mode_in_catalina)
- [DELL P4317Q monitor and Mac OS Catalina](https://discussions.apple.com/thread/250732856?answerId=251394294022)
- [Big Sur - Force RGB mode for displays](https://forums.macrumors.com/threads/big-sur-force-rgb-mode-for-displays.2268099)
- [Force RGB mode using Big Sur](https://www.reddit.com/r/MacOSBeta/comments/ipk1yn/force_rgb_mode_using_big_sur)



<!-- Refs -->

[patch-edid-src]: https://gist.github.com/adaugherity/7435890
