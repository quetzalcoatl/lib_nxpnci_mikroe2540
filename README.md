# Project

This project intends to clarify and simplify building and using Mikroe-2540 source code examples provided by NXP for Mikroe USB NFC Dongle.  
It does not mean "providing" nor "replacing" nor "improving" them, mostly due to licensing issues (see point 5 in *Mandatory preparations* below).  
This restriction of scope may change once I manage to find another SDK for Mikroe-2540, PN7150, (..) working on Windows.  
However, more probably, I'll just buy another NFC-capable dongle with better SDK.  
You've been warned :)

# Mandatory preparations

1. Find official code examples for Mikroe-2540, provided by NXP, and download them
  - vendors selling Mikroe-2540 often provide a direct link to those files, read the product description carefully
  - for example [MIKROE themselves do that](https://www.mikroe.com/nfc-usb-dongle) and they point to [this file on NXP site](http://www.nxp.com/documents/software/SW4335.zip)
  - since the file MAY change at NXP's discretion, it may be a nice idea to verify it. When I fetched it, it was:
    ```
    File: SW4335.zip
    Size: 2124977 bytes
    SHA1: 39D5CF1E9DB260851274E338CDA55D4A71E70D4A
    ```
2. Unpack following files from `SW4335.zip`:
  - `NXP-NCI_Windows_example.exe`
  - `NXP-NCI_example_v1.0.zip`
3. Unpack `NXP-NCI_example_v1.0.zip`
4. Find **licensing information** in files you've just unpacked. When I fetched it, it was:
    ```
        File: NXP_SLDA.pdf
        Size: 179188 bytes
        SHA1: CC08CEF0846B3F0D23C2EC5318692D68959F22D6
    ```
5. Read the licensing carefully, and maybe agree to the terms.
  - **Unless licesing information has changed, reading it should make it clear:**
    - why I cannot just publish the full source code of lib_nxpnci_mikroe2540 on GitHub
    - why I cannot publish `SW4335.zip` for Mikroe-2540
    - why you had to find a link to `SW4335.zip` on vendor's site
    - why the vendor could not publish `SW4335.zip` and had to publish a link to NXP's site
    - why you had to download `SW4335.zip` by yourself, from NXP, and maybe agree to the terms before using it
6. ⭕If you **do not agree** to follow those licensing terms, well..  
   Find a way to obtain `SW4335.zip` examples under a different license.  
   Or assume this project is not suitable for you due to licensing and find another SDK for Mikroe-2540.  
   Either way, if you succeed, please let me know!
7. ⭕There is a slight chance that I misunderstood the license.  
   In such case, I may have to strip information and/or resources contained in this repository even further, or even take this repository down.  
   Consider now-famous case of [youtube downloader project being taken down](https://en.wikipedia.org/wiki/Youtube-dl).  
   Things happen when IP lawsuits begin to show up on the horizon.
8. Verify that `NXP-NCI_Windows_example.exe` works with your dongle.  
  ⭕ **If it does not work, then probably the code will not work either.**  
  If you've got problems running this demo app, or connecting Mikroe-2540 to your computer, etc, `QuickStartGuide_Windows.pdf` has some general hints.
9. Remember `NXP-NCI_Windows_example` folder from `SW4335.zip`.  
    This is the source code of the demo application you've seen already, `NXP-NCI_Windows_example.exe`.  
    This folder also contains a nicely separated subfolders with proprietary libraries that the demo application uses for talking to Mikroe-2540.  
    I like to call them `lib_nxpnci_mikroe2540`.

# Building as a demo application

1. Clone/copypaste/whatever to get sources of this project to your computer
2. Find directory called `lib_nxpnci_mikroe2540`
3. Copy whole `NXP-NCI_Windows_example` folder as it is into; afterwards the directory with SDK sources should sit in the same directory as `lib_nxpnci_mikroe2540.vcxproj`
4. Create a solution, open your existing solution, etc
5. *Add Existing Project* to your solution: `lib_nxpnci_mikroe2540\lib_nxpnci_mikroe2540.vcxproj`
6. For now, leave `lib_nxpnci_mikroe2540.vcxproj.props` as it is.
7. Try building just this project. It should succeed and produce a console application, `lib_nxpnci_mikroe2540.exe`, that should behave similar to `NXP-NCI_Windows_example.exe` from the examples.

# Building as a static library

1. Complete [building demo app steps](Building-as-a-demo-application)
2. Open properties of `lib_nxpnci_mikroe2540` project
3. (optional) Ensure `Configuration` filter is set to `All Configurations`
4. (optional) Ensure `Platform` filter is set to `All Platforms`
5. Change `General\Configuration Type` to `Static library (.lib)`
6. Adjust any other flags as needed, see [details](Compile-time-configuration)
7. 🔶 Reload Projects or Solutions to make sure VS refreshed all options and file lists *)
8. Try building just this project. It should succeed and produce a static library, `lib_nxpnci_mikroe2540.lib`

*) Yeah, it sounds strange. You've just used project's Property-Pages to just switch ConfigurationType. Usually, VS handles it very well. However, `lib_nxpnci_mikroe2540.vcxproj` contains a few `Condition`s **) based on `ConfigurationType` that VS2019 does not handle well. These conditions include/exclude some files to/from the project, and VS seems oblivious to that. If you don't unload/reload the project, VS won't update file lists properly.

**) Yes, I know that it's possible to use `Configuration|Platform` for that. I chose Conditions because, let's face it, in 99% of the cases we won't be switching from staticlib back to demoapp, ever, and the extra build configurations would be just a dead weight.

# Building as a dynamic library

Not supported/tested/tried/prepared at this moment.

TODO

# Using static library

1. After you've successfully build this project as static library, it's ready to be linked to your other projects. Keep in mind the licensing terms of course.
2. Add following line to your `vcxproj` file, and it should take of all the boring details. Adjust the path to match your solution structure.

    ```xml
    <Import Project="..\lib_nxpnci_mikroe2540\lib_nxpnci_mikroe2540.vcxproj.props" Condition="exists('..\lib_nxpnci_mikroe2540\lib_nxpnci_mikroe2540.vcxproj.props')" />
    ```

3. In case you can't add this line to your `vcxproj`, or in case it doesn't work for some reason, you need to add `NfcLibrary\inc` folder from `NXP-NCI_Windows_example` to your include-search-paths, and add `lib_nxpnci_mikroe2540.lib` to linking phase, and add `SetupAPI.lib` from winsdk to linking phase.
4. `#include <Nfc.h>` where needed

# Compile-time configuration

Configuration options for `lib_nxpnci_mikroe2540` in form of -D/#defines reside in `lib_nxpnci_mikroe2540.vcxproj.props`.  
By default, all options are set to their default state as in `SW4335.zip`, as seen in `.cproject` Eclipse project file.

Just comment/uncomment relevant lines. Below you can find a short summary of options and their effects, if effects are known.

Also [check out AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf). While it doesn't refer to Mikroe-2540, a lot of information contained there still applies, like compilation flags, demo app scenarios, etc. Use with care though, as that AN refers to a different board cooperating with a very similar 'examples' code.

## File/version selection
- `UseUpdatedLpcusbsio`: false|true, default: false  
  When `false`, builds original code from `SW4335.zip`, except for `lpcusbsio.c` (see other notes below for details).  
  When `true`, project uses updated `lpcusbsio` from `Linux NFC Stack`  
  Keep in mind that updated `lpcusbsio` requires `pthread.h` to be available, probably the easiest way is via `vcpkg` [see also here](https://stackoverflow.com/a/63266134/717732). Also, I've had to adjust it a bit to actually compile on VS2019 compiler.  

🔶Note: it's best to reload project/solution after changing this flag to force VisualStudio to reload project file lists. *) and **) apply here as well.

## Support for Card Emulation mode
- `CARDEMU_SUPPORT`: #define flag
  Enables support for Card Emulation mode.  
  Enables Card Emulation scenarios when building as the demo app.

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 12 para 4.3

## Support for P2P, Peer-to-Peer mode
- `P2P_SUPPORT`: #define flag
  Enables support for P2P communication mode.  
  Enables P2P scenarios when building as the demo app.

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 10 para 4.2, and also page 21 para 6.10

## Support for RW, Tag Reader-Writer mode
- `RW_SUPPORT`: #define flag  
  Enables support for acting as a NFC Tag Reader-Writer mode.  
  Enables RW scenarios when building as the demo app.

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 10 para 4.1

## Support for NDEF
- `NO_NDEF_SUPPORT`: #define flag  
  A curiosity. It effectively disables most of RW features and reduces the `RW_SUPPORT` to just discovered/gone notification with no data transfer. I don't understand the reason for this compile-time option. I've tried tracking what actually is being disabled, and it may be possible that the PN7150 chip somehow assists in handling NDEF messages, and that some version of the chip may lack this features while providing support for other.. no idea, really.  
  Sadly, AN11990 is silent on that matter as well.

## Options specific to the Demo App

- `RW_NDEF_WRITING`: #define flag  
  Has effect only when communication ends up using T1,T2,T3,T4/ISODEP protocol.  
  Ignored when `RW_RAW_EXCHANGE` is enabled.  
  Instructs the demo app to try writing some test NDEF data to the NFC tag.  
  🔶That means that current data present on the tag may be damaged.

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 20 para 6.8 for a quick overview of that scenario.

- `RW_RAW_EXCHANGE`: #define flag  
  Disables NDEF read/write operations in T1,T2,T3,T4/ISODEP.  
  Enables scenarios listed below.
  - PCD-MIFARE for Mifare
  - PCD-ISO-15693 for ISO-15693
  - PCD-ISO-14443-3A for T2 protocol
  - PCD-ISO-14443-4 for T4/ISODEP protocol

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 19 para 6.6 for a quick overview what each scenario presents.  
  Each of those scenarios, except T4/ISODEP, tries writing something to the NFC tag.  
  🔶That means that current data present on the tag may be damaged.

- `CARDEMU_RAW_EXCHANGE`: #define flag  
  Disables NDEF operations in Card Emulation mode.  
  Enables scenarios listed below.
  - PICC_ISO14443_4 for T4/ISODEP

  See [AN11990](https://www.nxp.com/docs/en/application-note/AN11990.pdf) page 20 para 6.7 for a quick overview of that scenario.

## Hex-dumping packets to console
- `NCI_DEBUG`: #define flag  
  Enable hexdumps from NxpNci layer.

- `HID_DEBUG`: #define flag  
  Enable hexdumps from HID layer.

## Building with DDK
- `HIDAPI_USE_DDK`: #define flag
  If not defined, project uses local definitions of some structures and `hid.dll` from the OS.  
  If defined, project relies on `#include <hidsdi.h>` from the DDK instead.

## HIDAPI as DLL
- `HID_API_EXPORTS`: #define flag  
  Mark `hidapi.h` functions with `__declspec(dllexport)`

## LpcUsbSIO as DLL
- `LPCUSBSIO_EXPORTS`: #define flag
- `LPCUSBSIO_IMPORTS`: #define flag  
  Mark `lpcusbsio.h` functions with `__declspec(dllexport)` or `__declspec(dllimport)`

## Other flags
- `PICPGM`: #define flag
- `S11`: #define flag
- `P32`: #define flag  
  First two are of unknown meaning. Last one is enabled by default, hardcoded within TML\src\hid.c lines 867-869

# Other notes, relations to other SDKs, etc

Code example in form of `SW4335.zip` from NXP for Mikroe-2540 has rather simple layered architecture.  
Layers are easily seen just by the folder structure, but it's even more layered than just what's seen from folders.  
Inside you can find, amongst others:
- a handful of general-purpose utilities by *Eff'Innov Technologies* (Apache License, v2.0),
- a HIDAPI library by *Alan Ott* from *Signal 11 Software* (http://github.com/signal11/hidapi, licensed very permissively, do see! Thank you Alan!),
- `lpcusbsio` and `lpcusbsio-i2c` ("LPC USB serial I/O interface definition" and "Protocol definitions for LPCUSBSIO's I2C interface" respectively), both by NXP (and both on a bit less restrictive license terms, comparing to `SW4335.zip`).

Since Mikroe-2540 is based on LPC chip, it may be possible to communicate with it via [nxpUSBLib](https://github.com/openxc/nxpUSBlib). I have not tried yet.

Googling for `lpcusbsio` revealed that LPC UBS IO is a common protocol/library that may be used to communicate with a variety of NXP devices, and the I2C version we have here is obviously for talking with PN7150 via LP11U24F acting as USB-HID-to-I2C bridge. See for example [this document from NXP Community](https://community.nxp.com/pwmxy87654/attachments/pwmxy87654/lpcware-archive/66/1/lpcusbsio_v1_00.pdf).

Although library `lpcusbsio` seems to be available separately, I managed to find only one similar version, [v1.00 version at NXP Community](https://community.nxp.com/t5/LPCware-Archive-Content/LPC-USB-Serial-I-O-Library/ta-p/1129296), and it's a bit different. It looks very similar to the one provided with examples, but the version from examples contains extra patches and features not present in v1.00, so that v1.00 is surely an older version.

Mikroe-2540 is based on NXP `PN7150`. At the time of writing this, the first thing that sticks out on search engines is open-source [Linux NFC stack for NCI based NXP NFC Controllers (PN7150, PN7120)](https://github.com/NXPNFCLinux/linux_libnfc-nci). This project doesn't seem to support Windows environments, while the examples from NXP are provided for Windows, Linux and MacOS. I didn't put much effort though, so I may be wrong about it.

The example in form of `SW4335.zip` at first glance seems to have no relation to that project. However, `Linux NFC stack` is quite interesting, because we can find inside:

- a much newer version of [TML library](https://github.com/NXPNFCLinux/linux_libnfc-nci/tree/master/src/halimpl/pn54x/tml), licensed on Apache License v2.0, so much 'nicer' license than the TML included in `SW4335.zip`; the TML library has been changed in many places, making it hard to incorporate this newer version into code examples from `SW4335.zip`
- a Linux-based version of [HIDAPI](https://github.com/NXPNFCLinux/linux_libnfc-nci/blob/master/src/halimpl/pn54x/tml/lpcusbsio/hid/hidapi.h), as opposed to Windows-based version included in `SW4335.zip`. However, only header file `hidapi.h` differs. The implementation file `hid.c` is 100% identical, so there's no point in incorporating it into code examples from `SW4335.zip`
- a SLIGHTLY newer version of [lpcusbsio and lpcusbsio_i2c](https://github.com/NXPNFCLinux/linux_libnfc-nci/tree/master/src/halimpl/pn54x/tml/lpcusbsio/lpcusbsio), most of it is identical to what is provided in `SW4335.zip`, only `lpcusbsio.c` contains some important memory management fixes. Because of that, it is provided here in this repository, and by default the `vcxproj` is set to ignore the original `lpcusbsio.c` from `SW4335.zip` and use the newer one instead; other updated files from `lpcusbsio` are provided as well, and can be used instead of original files via a compile-time switch when building `lib_nxpnci_mikroe2540`
- and the `NxpNci library`. Code example for Mikroe-2540 carry their own NxpNci library, much older than the one in `Linux NFC Stack`.

This `Linux NFC Stack`, while its implementation differs greatly, has some similarities other than just the TML & lpcusbsio. For example, simple text search can easily reveal [similar #defines](https://github.com/NXPNFCLinux/linux_libnfc-nci/search?q=NXP_CORE_CONF_EXTN), so maybe the `SW4335.zip` is completely obsolete and that it may be replaced with new code based on `Linux NFC Stack` ... on Windows ... somehow :)