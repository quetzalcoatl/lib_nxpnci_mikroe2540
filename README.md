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
6. 🔴If you **do not agree** to follow those licensing terms, you're on your own.  
   Find a way to obtain `SW4335.zip` examples under a different license. Otherwise, best to assume this project is not suitable for you due to licensing and find another SDK for Mikroe-2540.  
   Either way, if you succeed, please let me know!
7. 🔴There is a slight chance that I misunderstood the license.  
   In such case, I may have to strip information and/or resources contained in this repository even further, or even take this repository down.  
   Consider now-famous case of [youtube downloader project being taken down](https://en.wikipedia.org/wiki/Youtube-dl).  
   Things happen when IP lawsuits begin to show up on the horizon.
8. Verify that `NXP-NCI_Windows_example.exe` works with your dongle.  
  🔴 **If it does not work, then probably the code will not work either.**  
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
7. (optional) Reload Projects or Solutions to make sure VS refreshed all options and file lists
8. Try building just this project. It should succeed and produce a static library, `lib_nxpnci_mikroe2540.lib`

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

TODO - begin

File/version selection
- UseUpdatedLpcusbsio  
  Default: `false`, builds original code from `SW4335.zip`, except for `lpcusbsio.c` (see other notes below for details). When `true`, project uses updated `lpcusbsio` from `Linux NFC Stack`; keep in mind that it requires `pthread.h` to be available - probably the easiest way is via `vcpkg` [see also here](https://stackoverflow.com/a/63266134/717732)  
  Note: it's best to reload project/solution after changing this flag to force VisualStudio to reload project file lists.

Card Emulation mode support
- CARDEMU_SUPPORT
- CARDEMU_RAW_EXCHANGE

P2P, Peer-to-Peer mode support
- P2P_SUPPORT

Tag Reader-Writer mode support
- RW_SUPPORT
- RW_NDEF_WRITING
- RW_RAW_EXCHANGE

Disable NDEF support (disables most of RW features)
- NO_NDEF_SUPPORT

Spam-a-lot
- NCI_DEBUG
- HID_DEBUG

Use HID definitions from DDK; uses local definitions if not enabled
- HIDAPI_USE_DDK

Mark hidapi.h functions with __declspec(dllexport)
- HID_API_EXPORTS

Mark lpcusbsio.h functions with __declspec(dllexport) or __declspec(dllimport)
- LPCUSBSIO_EXPORTS
- LPCUSBSIO_IMPORTS

Unknown, P32 is hardcoded as 'enabled', see TML\src\hid.c	lines 867-869
- PICPGM
- S11
- P32

TODO - end

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