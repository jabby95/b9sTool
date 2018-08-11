#### Instructions

https://3ds.hacks.guide/<br>
- WARNING: Never, under any circumstance, use this homebrew in conjunction with a youtube/video guide of any kind!<br>
- NOTE: I would recommend that anyone 11.4 - 11.7 inclusive use smea's arm9 hax that was revealed aug 11th, 2018 rather than seedminer based methods.

#### General Info

- This app only writes to FIRM0, not FIRM1, so it should be safe given your FIRM1 is not corrupt or a9lh'd.<br>
- Never use this on anything but 3DS FIRM 11.8.X-YZ.<br>
- NEVER use this if arm9loaderhax or sighax is installed (on any firmware)! The result will be a brick!<br>

Compiling: 
- Just supply the current decrypted FIRMs for both new/old 3ds and put them in the firm_new and firm_old 
directories respectively. Then place the newest boot9strap.firm payload in the payload directory. Then compile with
make FIRM_INFO="11.8 only" or whatever is actually the firmware range of the current native firm.
The user does not have to supply any extra files like in previous versions. Needs the latest libfat version.

### Important b9sTool 4.X.X differences to 2.X.X

BACKUP.BIN:<br>
- When b9sTool 4.X.X boots the first time, it will initialize BACKUP.BIN on the sd card. First, it executes the known-plaintext attack and writes the b9s payload to boot9strap/BACKUP.BIN.<br>
Then, it copies a clean firm backup to the same file. If you have clean firm installed, "install boot9strap" will be offered. If you have b9s installed, "restore stock firmware" will be offered.<br>
There is no F0F1 file in this version. Firm0 is backed up though, don't worry.<br>
Every session forward loads the same data from BACKUP.BIN (i.e. the b9s payload is not recalculated each time).<br>
Each time you use the app, it cycles between B9S and STOCK firms. BACKUP.BIN does not change until you reset the BACKUP.BIN.<br>
NEVER alter BACKUP.BIN manually in ANY way!

- BACKUP.BIN structure:<br>
0 - 0x100000 		Encrypted clean firmware backup<br>
0x100000 - 0x7800	Encrypted b9s payload firm<br>
0x10FE00 -0x110000	Plaintext Header with hashes for previous 2 regions<br>

- The plaintext header above is also written to the last sector of the NAND firm1 region for verifying the BACKUP.BIN.<br>
(this is to protect BACKUP.BINs from getting mixed up with the wrong systems, and being used across firmware versions - bad)<br>

Firmware info:<br>
- 4.0.1 - 4.1.2 is for firmware 11.8 ONLY (always check the firmware in the app menu to be sure as this can change as new firmwares are released!)<br>
- 4.0.0 is for firmwares 11.4 to 11.7 ONLY<br>
- Be careful using b9sTool between firmware updates -  the app should only allow you to restore B9S after it detects a native firm update. Restoring stock (i.e. downgrading your native firm with respect to the rest of the firmware titles) is a pretty bad idea!<br>

### TROUBLESHOOTING!!

A. If you get an a9lh error.<br>
- First, make sure you really don't have a9lh. If you use b9sTool with a9lh, you will brick.
If you're sure this is a false positive: rename boot9strap/key.bin (this is your console unique NAND header) to danger_skip_a9lh.bin
Then rerun b9sTool and the error should be skipped and you get to the menu. Unlike other override files, this one will stay persistant and not be deleted.

B. The first time you run b9sTool, you get BACKUP.BIN verify errors of some sort.
- Make sure BACKUP.BIN is in the boot9strap folder. Don't move it around or rename it. Moving/renaming it will not cause the file to regenerate. Just don't ever touch it.
Rename boot9strap/key.bin to danger_reset_backup.bin. Then rerun b9sTool and hopefully the menu shows up.

C. You select "Install boot9strap", reset the system, but the system just boots up normal.
- Boot b9sTool and select "Restore Stock Firmware". Turn off system then restart.
Go to System Settings > Other Settings > Page 4 > System Update. Make sure to actually attempt to update with wifi on.
It should say "your system is up to date". Press ok and let the system fully reboot to home menu on its own. Then turn it off.
Boot b9sTool again and hold X + LeftDpad for 5 seconds. A red 3rd menu option should appear for you to reset the backup. Select it. This will only work if FIRM STATUS is STOCK.
After the backup.bin resets, choose "Install boot9strap", and hopefully you will see luma3ds config on next reboot.

#### Credits

+ zoogie
