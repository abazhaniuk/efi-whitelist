Known (U)EFI Executables
========================

THE JSON FILES ARE PROVIDED 'AS IS' WITH ALL FAULTS. MCAFEE MAKES NO REPRESENTATIONS OR WARRANTIES, AND DISCLAIMS ALL REPRESENTATIONS AND WARRANTIES, EXPRESS OR IMPLIED, AS TO THE USE, PERFORMANCE OR ACCURACY OF THE JSON FILES. THE JSON FILES MAY HAVE ERRORS OR DEFECTS, AND ANY USE IS AT THE USER'S OWN RISK.

This repository contains information about known EFI executable binaries extracted from (U)EFI firmware update images gathered from platform vendors' web-sites. 

This information includes attributes of executable sections of EFI firmware:
 * SHA-256 and SHA-1 hash values of executable section
 * GUID of an EFI binary
 * name (UI string) of an EFI binary if available
 * type of executable section if different from PE/COFF (assumed PE/COFF if type is missing)

The information is presented in the following JSON format to make it easy to consume by any software:

    "f9d6c23a6d6b6daa3d28236cff978ee285d8aa9001e4985d18ea21849998c9dc": {
      "guid": "143D3B30-E95E-4D4C-8B0B-7918CCB2F84B",
      "sha1": "f9bd8bf7a5958dff2db8438eefb496a6172dc25f",
      "name": "VPROSMBIOS"
    },

There are multiple `efi_<vendor>.json` files in the repository containing EFI binaries extracted from firmware images of different platform vendors.

These JSON files can be used as a source of known EFI executables present in vendor firmware update images.
These files can be used with open source [CHIPSEC framework](https://github.com/chipsec/chipsec) to validate contents of firmware on supported systems against a list of known EFI executable binaries.

The following example illustrates how to use JSON files from this repository with CHIPSEC:

 * This command will automatically dump firmware image from ROM and validate it against a list of known EFI executables by specific vendor (`efi_vendor.json`)

    chipsec_main -m tools.uefi.whitelist -a check,efi_vendor.json

 * These two commands will dump firmware image from ROM to firmware.bin file and validate it against the same `efi_vendor.json`

    chipsec_util spi dump firmware.bin
    chipsec_main -m tools.uefi.whitelist -a check,efi_vendor.json,firmware.bin

[tools.uefi.whitelist](https://github.com/chipsec/chipsec/blob/master/chipsec/modules/tools/uefi/whitelist.py) module can also generate a list of EFI executables on any given system which can then be added to JSON files in this repository.

# Important

The lists of EFI executables in this repository are not complete and will likely have false positives, i.e. good EFI executables missing from the list flagged by tools.uefi.whitelist module. This happens due to a number of reasons:
 * Not all vendors have information about their firmware added to this repositry. Please make sure that `efi_<vendor>.json` file exists for your `<vendor>`.
 * Update images often don't have all EFI executables present in full firmware images on a system. In this case, full list can be created by adding a list of EFI executables generated from firmware image extracted from a clean system. It may be possible to request a full clean firmware image from the vendor
 * Firmware images for some systems may be missing on a vendor's web-site. Similarly to the previous case, missing image may be extracted from a clean system or requested from the vendor.
 * We were not able to extract and parse firmware images from a vendor's firmware updates. Please submit an issue as explained below and we'll try to fix this

If most of the EFI binaries on your system are missing from the list, then most likely the firmware for your system is missing from this repository. In this case, please consider submitting an issue with information about vendor/model and a link to firmware update image on vendor's web-site, if available.

We encourage platform vendors to contribute details about the contents of their clean firmware images to this repository.

Contents of JSON files are **known** EFI executables extracted from vendor (U)EFI firmware images. However, they may contain insecurely designed or vulnerable executables or 3rd-party executables used by vendor firmware.

# Current contents

**Unique (U)EFI firmware images: 13480**

**Unique EFI executables: 1910649**

Manufacturer | JSON file(s) | Number of EFI executables
------------ | ------------ | -------------------------
Acer | efi_acer.json | 37292
ASRock | efi_asrock.json | 26168
ASUS | efi_asus.json, efi_asus.1.json | 559857
Dell | efi_dell.json | 485970
Gigabyte | efi_gigabyte.json | 168119
HP | efi_hp.json | 102631
Intel | efi_intel.json | 106924
Lenovo | efi_lenovo.json | 166212
MSI | efi_msi.json | 271365
**Total** | | **1910649**


# Acknowledgements

* Oleksandr Bazhaniuk
* Yuriy Bulygin
* Andrew Furtak
* Prashant Krishnan Iyer
* John Loucaides
