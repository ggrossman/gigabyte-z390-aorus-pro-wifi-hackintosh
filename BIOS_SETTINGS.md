# BIOS Configuration

While I based my build mostly on cmer's AORUS Master repo, these BIOS settings are borrowed from
https://github.com/blacklizard/gigabyte-z390-aorus-pro-wifi-hackintosh-opencore/blob/master/BIOS.md

BIOS version: `F12c` (initial install on `F11`)

- Load Optimized Defaults
- Settings -> Internal Graphics -> Enabled
- Settings -> DVMT Pre-Allocated -> 32M
- Settings -> Wi-Fi -> Disabled
- Settings -> Above 4G Decoding -> Enabled
- Settings -> Wake on LAN Enable -> Disabled
- Settings -> USB Configuration -> Legacy USB Support -> Disabled
- Settings -> USB Configuration -> XHCI Hand-off -> Enabled
- Settings -> Software Guard Extensions(SGX) -> Disabled
- Settings -> Trusted Computing -> Security Device Support -> Disabled
- Boot -> CSM Support -> Disabled

## Important for Sleep/Wake

For weeks, my system was crashing after Sleep with "Sleep Wake Failure on EFI" panics.

I tried numerous things to fix it. Finally, I happened upon these BIOS settings:

- Settings -> Platform Power Management -> Enabled
- Settings -> PCH ASPM -> Enabled
- Settings -> PEG ASPM -> Enabled
- Settings -> DMI Link ASPM -> Enabled

Once I enabled these, Sleep/Wake worked as expected.

## Hidden BIOS setting

NOTE: While I did execute `setup_var 0x5C1 0x00` on my build, I still have `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` set
in config.plist. Also I haven't included modGRUBShell.efi in my config.plist.

I'll revisit this later based on https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#what-is-cfg-lock

This setting can be modified using [modGRUBShell.efi](https://github.com/datasone/grub-mod-setup_var/releases/download/1.1/modGRUBShell.efi)
⚠️: These offset are only valid for the specific firmware version! Do not try to execute these commands on a different firmware! 
The modified variables will reset itself when you restore / revert to optimised defaults. 
Everytime BIOS is reset, you will need to reconfigure the hidden setting or else you will not be able to boot, you have been warned

#### Required setting
- Disable CFG Lock

| Firmware Version | Command              |
|------------------|----------------------|
| F9, F10, F11 & F12c         |`setup_var 0x5C1 0x00`|

#### Optional Setting
- Turn off motherboard LED

| Firmware Version | Command               |
|------------------|-----------------------|
| 11               |`setup_var 0x16FE 0x00`|
|                  |`setup_var 0x16FF 0x00`|
|                  |`setup_var 0x1700 0x00`|
|                  |`setup_var 0x1701 0x00`|
