# MSI-MAG-B460-TORPEDO-EFI

There are few up-to-date Hackintosh guides for the MSI MAG B460 TORPEDO. This repository shares a working basic OpenCore EFI as a reference for other owners, with notes on resolving USB 3.0 discovery and mapping issues on the Intel A3AF controller.

## Hardware

| Component | Model |
|---|---|
| Motherboard | MSI MAG B460 TORPEDO (MS-7C81) |
| Chipset | Intel B460 |
| CPU | Intel Core i5-10400F (Comet Lake-S) |
| GPU | AMD Radeon RX 5500 XT (Navi 14) |
| Ethernet | Realtek RTL8125 2.5GbE |
| Wi-Fi | Broadcom 802.11ac — 14E4:43A0 |
| Audio | Realtek ALC1200A |
| USB Controller | Intel XHCI — 8086:A3AF |

## Compatibility and setup

This is a basic OpenCore EFI for the MSI MAG B460 TORPEDO. With minor changes to kexts, OpenCore version and macOS-specific settings, it can be used as a base for installations up to macOS Tahoe.

It is not a guaranteed plug-and-play configuration for every system. Review the settings for your hardware and target macOS version. The `SystemSerialNumber`, `MLB`, `SystemUUID` and `ROM` fields in `EFI/OC/config.plist` are blank for privacy; generate your own SMBIOS identifiers before use.

## USB 3.0 / A3AF Mapping Notes

USB mapping was the most time-consuming part of this setup.

- macOS identified the Intel XHCI controller as `8086:A3AF`.
- Initially, USB 3.0 ports were not detected correctly.
- On some ports, USB 3 devices were detected as USB 2.0, while SuperSpeed ports were missing or inactive in Hackintool.
- Ports did not map correctly in Hackintool.
- In `USBInjectAll.kext`'s `Info.plist`, the Intel controller match was manually changed from **A2AF to A3AF**.
- `XHCI-unsupported.kext` from the [daliansky/OS-X-USB-Inject-All fork](https://github.com/daliansky/OS-X-USB-Inject-All) was used.
- After these changes, USB 3 ports started working at **5 Gb/s**.
- Once all physical ports had been discovered, the final USB port map kext was created using Hackintool.
- Temporary mapping and injection kexts were removed for final use.

## Sources

- [OpenCore](https://github.com/acidanthera/OpenCorePkg)
- [Dortania OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
- [Dortania USB mapping / post-install guide](https://dortania.github.io/OpenCore-Post-Install/usb/intel-mapping/intel.html)
- [USBInjectAll / XHCI-unsupported fork used during troubleshooting](https://github.com/daliansky/OS-X-USB-Inject-All)
- [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)
