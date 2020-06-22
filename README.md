# Asus Vivobook X507UA

Catalina 10.15.5 (Clover) and 10.15.5 (OpenCore) 

## System specification

1. Name:           Asus Vivobook X507UA
2. CPU:            Intel Core i3-7020U
3. Graphics:       Intel UHD Graphics 620 
4. Wifi:           Qualcomm Atheros AR9565
5. Card Reader:    Connected via USB
6. Camera:         ASUS UVC HD
7. Audio:          ALC256
8. Touchpad:       ELAN1200
9. Bios Version:   306

## Thing will not able to use

1. FN + media controller's key

## VoodooI2C

1. Polling mode for smooth movements and gestures
2. Finger ID-implemented VoodooInput and VoodooI2C for persistent gesture input


## Activate Sleep and Airplane fn keys

1. Download Release of [AsusSMC](https://github.com/hieplpvip/AsusSMC/releases).
2. Run install_daemon.sh by dragging it onto terminal.
- Note: Reboot if the script does not seem to work. After updating your EFI folder or any kexts, you may need to rerun the script.

## Remap PS2

1. Use MaciASL to save ACPI/additional/SSDT-PS2.dsl with the .aml extension in Patched folder.
2. Reboot.
- Optional: If you have a non-macOS USB keyboard, uncommenting the code in SSDT-PS2.dsl will swap left-cmd and left-alt. Install and configure Karabiner-Elements to switch back left-cmd and left-alt. This result in the same mapping in your PS2 keyboard and USB keyboard.

## Replace XOSI patch
Pick one of the below two patches.
### Insert _OSI for "Darwin"
1. Delete ACPI/Patched/SSDT-XOSI.aml.
2. Use MaciASL to save ACPI/replacement/SSDT-_OSI-XINI.dsl with the .aml extension in Patched folder.
3. Delete the _OSI and OSID patch in config.plist/ACPI/DSDT/Patches and copy OSYS patch from /replacement/config-_OSI-XINI.plist/ACPI/DSDT/Patches to config.plist.
4. Reboot.
### Assign OSYS "Windows 2015" value
1. Delete ACPI/Patched/SSDT-XOSI.aml.
2. Use MaciASL to save ACPI/replacement/SSDT-OSYS.dsl with the .aml extension in Patched folder.
3. Reboot.
- In my opinion, SSDT-OSYS.dsl is safer than SSDT-_OSI-XINI.dsl.

## Unlock MSR 0xE2 (CFG Lock)
- Note: You need to know which BIOS version matches your laptop model. Otherwise, there may be a permanent damage to your laptop.
1. Follow [Dortania's guide](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/extras/msr-lock).
2. The offset will be at 0x527 if your BIOS version is 309.

## When you think you are done

1. Update Clover, kexts, and efi files, but make sure to delete VoodooInput.kext inside VoodooPS2Controller.kext.
2. Backup your /L*/E* by copying them to the system EFI partition and/or installation USB EFI partition.

## Other things

1. OpenCore
    - Load CC from /L*/E*
    - Booting Windows is OK if you use KMS license.
    - Need to configure BlessOverride
    - Download bootpicker and chime resources available at https://github.com/acidanthera/OcBinaryData .
2. Clover
    - If you can't get Fn keys to work (namely touchpad enable/disable), try loading all kexts except CC from Clover in which case BrcmFirmwareData needs to load instead of BrcmFirmwareRepo.
    - If you update kexts, you need to delete VoodooInput.kext in VoodooPS2Controller.kext/Contents/Plugins to avoid loading VoodooInput twice. VoodooInput.kext, which is required by VoodooI2C for MT2 emulation, is already bundled with VoodooI2C.

