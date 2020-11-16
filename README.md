1. Find official code examples for Mikroe-2540, provided by NXP, and download them
  - vendors selling Mikroe-2540 often provide a direct link to those files, read the product description carefully
  - for example [MIKROE themselves do that](https://www.mikroe.com/nfc-usb-dongle) and they point to [this SW4335.zip on NXP site](http://www.nxp.com/documents/software/SW4335.zip)
2. Unpack following files from `SW4335.zip`:
  - `NXP-NCI_Windows_example.exe`
  - `NXP-NCI_example_v1.0.zip`
3. Unpack `NXP-NCI_example_v1.0.zip`
4. Find **licensing information** in files you've just unpacked. When I fetched it, it was file called `NXP_SLDA.pdf`
5. Read the licensing carefully, and maybe agree to the terms.
  - **Unless licesing information has changed, reading it should make it clear:**
    - why I cannot just publish the full source code of lib_nxpnci_mikroe2540 on GitHub
    - why I cannot publish `SW4335.zip` for Mikroe-2540
    - why you had to find a link to `SW4335.zip` on vendor's site
    - why the vendor could not publish `SW4335.zip` and had to publish a link to NXP's site
    - why you had to download `SW4335.zip` by yourself, from NXP, and maybe agree to the terms before using it
6. 🔴If you **do not agree** to follow those licensing terms, you're on your own. Find a way to obtain `SW4335.zip` examples under a different license. If you succeed, be sure to let me know! Otherwise, best to assume this project is not suitable for you due to licensing and find another SDK for Mikroe-2540.
7. 🔴There is a slight chance that I misunderstood the license. In such case, I may have to strip information and/or resources contained in this repository even further, or even take this repository down. Consider famous case of [youtube downloader project being taken down](https://en.wikipedia.org/wiki/Youtube-dl).