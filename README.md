# Sierra-10.12.6 on i7-6700k, asus z170-p, asus GTX970, realtek alc887

## bios setup

disable fastboot, secure boot, enable AHCI, 

## install Sierra
1. install osx with unibeast usb drive installer.
2. before install, expend existing ESP (EFI system partition) to > 200M with DiskGenius.
3. during install, run disk utility, select Mac with Journel format the target drive, also name it as OSX, otherwise it will be called "--".
3. after install, boot from usb and select OSX drive. 
4. install multibeast to application folder, 
5. get latest Clover EFI bootloader (v2.4k r4200).pkg, run and select boot with UEFI, also on dirvers64UEFI select EmuVariableUefi-64.efi, and check run rc script on target drive.  This will allow Clover simulate the nvram on asus z170-p mobo, so that the nvidia web driver can be enabled.

## allow HD drive boot

under window partition, run bootice, add new boot menu item OSX and point to ESP:/EFI/Clover/CLOVERX64.efi
adjust order to 1st.

## GTX970 nvidia WebDriver

1. Lilu.kext and NvidiaGraphicsFixup.kext, put them into /Clover/kext/other
2. install latest nvidia gtx webdriver
3. open /Clover/config.plist with Clover Configurator, 
  on Boot section, select nvda_drv=1, 
  on System Parameters, check NvidiaWeb, InjectKexts Yes
  on SMBIOS, right side dropdown, select iMac 14,2
4. reboot
5. Now in Nvidia Driver Manager select web driver, reboot again

## A6210 Wifi USB
1. download kext utility / kext beast
2. install RT2870USBWirelessDriver.kext to /System/Library/Extensions with kext utility or kext beast.
3. install wireless utility MT7612_7610U_D5.0.1.25_SDK1.0.2.18_UI5.0.0.27_20151209.dmg
4. reboot

## ALC887 on board sound card
1. backup /System/Library/Extensions/AppleHDA.kext, so that can restore it with kext utility later if necessary.
2. /Clover/config.plist, 
   on ACPI section, add "Rename HDAS to HDEF" DSDT patch;
   on Devices section, Audio 1 Inject, (uncheck ResetHDA)
3. Multibeast, install Audio drivers ALC887 current and 100 / 200 Series Audio; Build and Install.
4. put SSDT-HDEF-1.aml to /Clover/ACPI/patched (SSDT patch) https://github.com/toleda/audio_ALCInjection/blob/master/ssdt_hdef/ssdt_hdef-1-100-hdas.zip
5. reboot
6. test with audio_codecdetect_v2.3.command (chmod 755 to excute https://github.com/toleda/audio_CloverALC/blob/master/audio_cloverALC-130.command.zip)

Onboard audio codec
Realtek: 0x10ec0887
Name: Realtek ALC887
Audio ID: 1

Valid audio codec, audio device and Audio ID; audio injection is working
Finished
7. AppleALC inject method doesn't work on z170-p.

