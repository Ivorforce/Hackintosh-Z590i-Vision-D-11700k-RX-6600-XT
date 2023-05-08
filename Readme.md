Hi, this is my OpenCore Hackintosh build.

## How to Use

I am not uploading kexts, drivers and the likes, but it should be easy to reconstruct my build from the documentation with reasonings I provide. 

- Set up BIOS (as detailed below), (or like [here](https://github.com/SchmockLord/Gigabyte-Z590i-Vision-D-11900k) or [here](https://www.tonymacx86.com/threads/guide-oc-monterey-z590i-gigabyte-vision-d-i9-11900k-amd-rx6600.317472/))
- Follow [Open Core Tutorials](https://dortania.github.io/OpenCore-Install-Guide/) until you get to the EFI parts.
- Copy `config.plist` from this repo, place it as `EFI/OC/config.plist`.
- Run [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) with model `MacPro7,1` on it (needs to be this model because of Secure Boot and DRM).
- Download Kexts as detailed below. If you make changes, use [ProperTree](https://github.com/corpnewt/ProperTree) cmd+r to reassemple your references in `config.plist`.
- Let me know if you fix one of the issues!

### References

- Similar Builds (thank you!)
	- [Same Mainboard, similar GPU, CPU](https://www.tonymacx86.com/threads/guide-oc-monterey-z590i-gigabyte-vision-d-i9-11900k-amd-rx6600.317472/): This is where I started.
	- [Same GPU, CPU, similar mainboard](https://www.tonymacx86.com/threads/success-gigabyte-z590-vision-d-11700k-rx-6600-xt.316601/)
	- [Same Mainboard, similar CPU](https://github.com/SchmockLord/Gigabyte-Z590i-Vision-D-11900k)
- General 11th generation Intel (Rocket Lake) information: [here](https://github.com/luchina-gabriel/BASE-EFI-INTEL-DESKTOP-11THGEN-ROCKET-LAKE) (repo) and [here]([this](https://github.com/dortania/OpenCore-Install-Guide/pull/343)) (unmerged OpenCore Tutorial PR)


## Hardware

[Complete Build on PCPartPicker](https://pcpartpicker.com/b/7gZZxr).

- Mainboard: [Gigabyte Z590i Vision D](https://www.gigabyte.com/Motherboard/Z590I-VISION-D-rev-10/sp#sp)
	- [Ethernet](https://www.intel.com/content/www/us/en/products/sku/184676/intel-ethernet-controller-i225v/downloads.html)
	- [WiFi / BlueTooth](https://www.intel.com/content/www/us/en/products/sku/189347/intel-wifi-6-ax200-gig/specifications.html)
- CPU: [Intel i7-11700k](https://www.intel.com/content/www/us/en/products/sku/212047/intel-core-i711700k-processor-16m-cache-up-to-5-00-ghz/specifications.html)
- GPU: [Radeon 6600 XT](https://www.amd.com/en/products/graphics/amd-radeon-rx-6600-xt)

## Features

### Hardware

- WiFi: Reliable, but on some launches the device is missing (requiring a relaunch)
- Bluetooth: Reliable, but don't deactivate it, it can't be reactivated.
	- AirDrop: Other devices can be seen, but it itself is invisible. Can't send files.
	- Handoff (shared clipboard, quick app switch etc.): Works.
	- Screen Mirroring: Can mirror to others, but is not itself visible.
- LAN: Not tested.
- USB-C Devices: Works.
	- USB / Bluetooth Wakeup: Works.
	- Not all USB Ports are mappable, because macOS supports fewer ports than the Gigabyte Z590i Vision D offers. Therefore, some do not work.
- Thunderbolt: Not Tested.
	- Video Output via Thunderbolt: Doesn't Work.
- iGPU: Doesn't work (11th gen not supported by Apple)
- Digital Sound (via USB): Works.
- NVMe Drive: Works.
- VDA DEcoding / Hardware Acceleration: Works.

### Apps

- Video Playback: Works.
	- Netflix (DRM!): Works.
	- Youtube: Works.
- Steam: Blackscreens on login window.
- Programming (Godot / XCode / PyCharm): Works.
- DAW (Bitwig) / Sound Editing: Works.
	- Native Access: Crashes with GPU problems.

## BIOS

Starting out with parameters according to [this guide](https://www.tonymacx86.com/threads/guide-oc-monterey-z590i-gigabyte-vision-d-i9-11900k-amd-rx6600.317472/) (and with reference to [this](https://github.com/luchina-gabriel/BASE-EFI-INTEL-DESKTOP-11THGEN-ROCKET-LAKE) and [this](https://github.com/dortania/OpenCore-Install-Guide/pull/343)).

Keep the [Gigabyte BIOS Manual](https://download.gigabyte.com/FileList/Manual/mb_manual_z590i-vision-d_1001_e.pdf) handy to find your way around the GUI.

- SATA AHCI Mode
- Internal Graphics Auto (11700k iGPU isn't supported in macOS, but Windows may make use of it)

- Above 4GB Decoding Enabled
- Thunderbolt Enabled
- Hyper-Threading Enabled

- VT-d Disabled (See [here](https://github.com/dortania/OpenCore-Install-Guide/pull/343))
- CSM Disabled
- TPM Disabled
- CFG Lock Disabled (See [here](https://github.com/dortania/OpenCore-Install-Guide/pull/343) and [here](https://github.com/luchina-gabriel/BASE-EFI-INTEL-DESKTOP-11THGEN-ROCKET-LAKE))
- Legacy USB Support Disabled (who needs this?)

Enable Secure Boot and config thunderbolt via [this](https://github.com/SchmockLord/Gigabyte-Z590i-Vision-D-11900k).
Remember to re-do Secure Boot on OpenCore updates!

## Documentation

### Useful for Changes

- Use [MountEFI](https://github.com/corpnewt/MountEFI) to mount the OpenCore USB EFI.
- Use [ProperTree](https://github.com/corpnewt/ProperTree) to automatically add ACPI, driver, kext and tools to config.plist
- Useful: [config.plist reference](https://dortania.github.io/docs/latest/Configuration.html)
- Use [this config sanity tool](opencore.slowgeek.com) to optimize entries without knowing absolutely everything
- When uploading a new version, remove PlatformInfo entries for privacy reasons.

### Changelog (old to new), starting from [here](https://www.tonymacx86.com/threads/guide-oc-monterey-z590i-gigabyte-vision-d-i9-11900k-amd-rx6600.317472/)

- (2023-05-06) Delete AGPMInjector (hardware specific file)
- (2023-05-06) Disabled Whatevergreen (blanked my screen)
- (2023-05-06) Updated OpenCore and kexts, fixing WiFi (?)
- (2023-05-06) Pin SecureBootModel to j185f according to [this guide](https://dortania.github.io/OpenCore-Post-Install/universal/security/applesecureboot.html#securebootmodel)
- (2023-05-06) Delete IntelBluetoothInjector, AppleMCEReporterDisabler, AirportBrcmFixup (not needed)
- (2023-05-06) Add e1000=0 to boot-args ([Reasoning Here](https://github.com/luchina-gabriel/BASE-EFI-INTEL-DESKTOP-11THGEN-ROCKET-LAKE))
- (2023-05-06) Replace OpenHFSPlus with HFSPlus driver (supposed to be faster)
- (2023-05-07) Spoof I225-V LAN device ID as I225LM (as per [this](https://github.com/luchina-gabriel/BASE-EFI-INTEL-DESKTOP-11THGEN-ROCKET-LAKE) and [this](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#deviceproperties)). Remove all the other entries. Now I am able to activate Whatevergreen - it was probably not working because of faulty entries in DeviceProperties.
- (2023-05-07) Remove XHCI-unsupported which is probably not needed.
- (2023-05-07) Remove SASMegaRAID which I don't need. I think this fixed Wi-Fi and Bluetooth stability too?
- (2023-05-07) Enable ExtendBTFeatureFlags for continuity.
- (2023-05-08) Add Secure Boot to BIOS, which (i think) fixed the Apple Store.
- (2023-05-08) Adapt ACPI from [here](https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-AsRock-Z490-Phantom-ITX-TB3) instead, which is less bloated.
- (2023-05-08) Use MacPro7,1 as spoof platform [as this fixes DRM](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.Chart.md) (Netflix etc.)
- (2023-05-08) Clean up unneeded kexts, and document the rest.

### OpenCore

Currently on `0.9.1`.

### ACPI (kindly copied from [here](https://github.com/SchmockLord/Gigabyte-Z590i-Vision-D-11900k))

- SSDT-EC-USBX: [Desktop USB EC Fix](https://dortania.github.io/Getting-Started-With-ACPI/Universal/ec-fix.html)
- SSDT-PLUG: [CPU Power Management](https://dortania.github.io/Getting-Started-With-ACPI/Universal/plug.html) from [here](https://www.tonymacx86.com/threads/guide-oc-monterey-z590i-gigabyte-vision-d-i9-11900k-amd-rx6600.317472/)
- SSDT-AWAX: Fix [System Clock](https://dortania.github.io/Getting-Started-With-ACPI/Universal/awac.html)
- SSDT-DTPG: Thunderbolt
- SSDT-MAPLE-RIDGE-RP05-V2: Thunderbolt
- SSDT-Disable-CNVW: No idea, but it's needed to launch.
- SSDT-USB-Ports: USB Port mapping
- SSDT-USBW: USB Wakeup Fix

### Kext

Note: I haven't tested if all are actually required, some tweaking may be useful.

- Lilu: Always required
- VirtualSMC: SMC Emulator, always required
- SMCSuperIO: Fan Monitoring
- SMCRadeonGPU: Radeon GPU temperature Monitoring
- SMCProcessor: CPU Temperature Monitoring
- WhateverGreen: Important graphics and DRM fixes
- AirportItlwm: WiFi and Bluetooth
- AppleALCU: Digital-Only Sound
- BlueToolFixup: Bluetooth card support
- IntelBluetoothFirmware: Bluetooth card support
- IntelBTPatcher: Bluetooth bug fixes
- NVMeFix: NVMe Support
- RadeonSensor: Radeon GPU Support
- RestrictEvents: Various event block fixes, e.g. for the used MacPro7,1 model
- USBToolBox.kext: USB Port Map Library
- UTBMap.kext: USB Port Map